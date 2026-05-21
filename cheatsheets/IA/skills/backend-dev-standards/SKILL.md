---
name: backend-dev-standards
description: >
  Apply personal backend development standards when building, reviewing, scaffolding,
  or auditing any server-side application (Node.js, Express, NestJS, FastAPI, or similar).
  Use this skill whenever the user asks to: create a new backend project, generate a controller
  or service, design an API endpoint, set up middleware, configure authentication, write
  migrations, set up logging, review code quality, structure a database schema, prepare a
  deployment checklist, or document an API. Also trigger when the user says "apply my standards",
  "follow my stack", "set up my backend", "use our conventions", or mentions REST API, endpoints,
  Swagger, JWT, rate limiting, or database migrations. This skill encodes the user's personal
  backend engineering methodology — always apply it even if the user does not explicitly ask.
---
 
# Backend Dev Standards
 
This skill encodes the user's personal backend engineering standards for Node.js, Express,
NestJS, FastAPI, and similar frameworks. Apply all relevant sections based on the technology
in use. Never skip a section without a documented reason.
 
---
 
## 1. DOCUMENTATION
 
Every project ships with these files before the first real commit:
 
### README.md — Developer onboarding (setup in under 5 minutes)
Must include:
- Project name + one-line description
- Problem it solves
- Badges (build status, coverage, version, license)
- Tech stack list
- Requirements (Node/Python version, package manager, env vars needed)
- Installation and execution commands (copy-paste ready)
- Test and deployment commands
- Link to official technology docs
- Author
### DOCS.md — Architecture reference
Must include:
- Annotated folder structure
- Architecture diagram (ASCII or linked image)
- Entity-Relationship diagram (if there is persistent data)
- Service architecture diagram (how services communicate)
- API contracts: method, endpoint, request/response shape
- Environment variables table (key, description, example value)
- Deployment steps
### CHANGELOG.md — Version history
Format: Keep a Changelog (https://keepachangelog.com)
Sections per version: Added, Changed, Fixed, Removed
 
---
 
## 2. ARCHITECTURE AND STRUCTURE
 
### Folder structure
```
src/
├── modules/          # Domain modules (auth, users, products)
│   └── users/
│       ├── controllers/
│       ├── services/
│       ├── models/
│       ├── types/
│       └── tests/
├── config/           # App config, env validation, constants
├── middleware/       # Global middleware (auth, logging, error handler)
├── utils/            # Pure helper functions
├── types/            # Global TypeScript types and interfaces
└── tests/            # Integration-level tests
```
 
### Layer separation — enforced, no exceptions
```
Router → Controller → Service → Repository/Model
 
Router:      defines routes and applies middleware
Controller:  handles HTTP in/out, delegates to service, no business logic
Service:     all business logic lives here
Repository:  all database queries live here
```
 
Skipping layers is technical debt. A controller that queries the DB directly is a violation.
 
### Path aliases
Configure in `tsconfig.json` and resolve in bundler:
- `@/config`
- `@/middleware`
- `@/services`
- `@/types`
- `@/utils`
---
 
## 3. API DESIGN
 
> **Full REST rules reference:** read `references/rest-api-rules.md` when designing
> or reviewing any endpoint. It covers all 50 rules: naming, pagination, idempotency,
> caching, deprecation, bulk ops, i18n, and when to use alternatives to REST.
 
### Core rules (apply always)
- All routes: `/api/v1/...` — breaking changes require `/api/v2/`
- Plural nouns, no verbs (`/users` not `/getUsers`). Kebab-case for compound names.
- Nested resources max 2 levels. URI = resource identity. Query params = filtering/sorting.
- GET has zero side effects. Never mutate state on a GET.
- Response DTOs separate from DB models — never serialize ORM entities directly.
- Every list endpoint is paginated. No unbounded collections.
- Dates always ISO 8601 UTC. Field naming consistent (pick camelCase or snake_case, never mix).
### HTTP status codes
| Code | When |
|------|------|
| 200 | Success with body |
| 201 | Created |
| 204 | Success, no body |
| 400 | Validation error |
| 401 | Not authenticated |
| 403 | Not authorized |
| 404 | Not found |
| 409 | Conflict |
| 422 | Unprocessable entity |
| 429 | Rate limit exceeded |
| 500 | Server error |
| 503 | Temporarily unavailable |
 
### Response contract — no exceptions
```ts
// Single resource
{ success: true, data: T }
 
// Collection
{ success: true, data: T[], meta: { page, limit, total, totalPages } }
 
// Error
{ success: false, error: { code: string, message: string, details?: unknown } }
```
 
### Swagger / OpenAPI
- Served at `/api-docs`, auto-generated, always in sync
- Every endpoint documents: params, body schema, all response shapes, auth method, limits
- At least one request/response example per endpoint
### Health check
`GET /health` — 200 only when DB, cache, and critical dependencies all pass
```ts
{ status: 'ok', services: { db: 'ok', cache: 'ok' }, uptime: number }
```
 
---
 
## 4. CLEAN CODE
 
- **Single Responsibility**: every function does one thing. One JSDoc line above it.
- **Strict typing**: TypeScript Strict Mode always on. `any` is banned.
- **No `console.log`** anywhere — all output goes through the logger.
- **Import order**: external → internal (`@/...`) → types. Blank line between groups.
- **Error handling**: never swallow errors silently. Always propagate or log + respond.
### JSDoc example
```ts
/** Finds a user by email and throws if not found */
const getUserByEmail = async (email: string): Promise<User> => { ... }
```
 
---
 
## 5. DATABASE
 
### Migrations
- Every migration has `up()` and `down()` — rollback is non-negotiable
- Never modify a migration already executed in production — create a new one
- Naming: `YYYYMMDDHHMMSS_description_action` (e.g. `20240315120000_add_users_email_index`)
- Seeds are separate from migrations (test data ≠ schema structure)
### Queries
- All queries go through the Repository layer — never in controllers or services
- Use parameterized queries — never string concatenation (SQL injection prevention)
- Index columns used in WHERE clauses and JOINs
---
 
## 6. SECURITY
 
- **Rate limiting** by IP on all public endpoints
- **CORS** with explicit origin allowlist — never `*` in production
- **Validation and sanitization** of body, params, and query before reaching the controller (use Zod, Joi, or class-validator)
- **Authentication**: JWT with short expiry + refresh token rotation, or API Key in `Authorization` / `X-API-Key` header
- **Env vars**: sensitive values in `.env` — never hardcoded in source
- `.env` and `.env.prod` always in `.gitignore`
- `.env.example` committed with all required keys and no real values
- **Startup validation**: if a required env var is missing, the server must not start
```ts
// config/env.ts — run before anything else
const required = ['DATABASE_URL', 'JWT_SECRET', 'PORT'];
required.forEach(key => {
  if (!process.env[key]) throw new Error(`Missing required env var: ${key}`);
});
```
 
---
 
## 7. MIDDLEWARE AND OPERATIONAL PRACTICES
 
### Global middleware stack (applied in this order)
1. Security headers
2. CORS
3. Request logger (logs method, path, timestamp, correlation ID)
4. Body parser + sanitizer
5. Rate limiter
6. Auth validator (on protected routes)
7. **Centralized error handler** (last middleware — catches everything)
### Centralized error handler
One place catches all errors, formats them, logs them, and responds:
```ts
// Always the last middleware registered
app.use((err, req, res, next) => {
  logger.error({ err, correlationId: req.headers['x-correlation-id'] });
  res.status(err.status || 500).json({
    success: false,
    error: { code: err.code || 'INTERNAL_ERROR', message: err.message }
  });
});
```
 
### Response helper
Centralize all HTTP responses — controllers never call `res.json()` directly:
```ts
respond.ok(res, data)
respond.created(res, data)
respond.notFound(res, 'User not found')
respond.error(res, 400, 'VALIDATION_ERROR', 'Email is required')
```
 
---
 
## 8. HEADERS
 
### Security headers (set via middleware like Helmet)
- `Content-Security-Policy`
- `X-Frame-Options: DENY`
- `X-Content-Type-Options: nosniff`
### Traceability headers
- `X-Request-ID` — unique ID per request (generate if not received)
- `X-Correlation-ID` — propagate across service calls for distributed tracing
### Rate limit headers (return on every response)
- `X-RateLimit-Limit`
- `X-RateLimit-Remaining`
- `X-RateLimit-Reset`
---
 
## 9. LOGGING
 
- Structured JSON logs with Winston or Pino
- Levels: `error`, `warn`, `info`, `debug`
- `console.log` banned in all environments
- Every request log includes:
```json
{
  "timestamp": "2024-03-15T12:00:00Z",
  "level": "info",
  "method": "POST",
  "path": "/api/v1/users",
  "status": 201,
  "duration": 43,
  "correlationId": "abc-123"
}
```
 
- Errors include full stack trace, never just the message
- Never log sensitive data: passwords, tokens, credit cards, PII
---
 
## 10. TESTING
 
- Coverage target: >80%
- Test behavior, not implementation details
- One test file per controller, service, and utility
- Structure: Arrange → Act → Assert
- Integration tests hit the actual HTTP layer (use Supertest or httpx)
### What to test at minimum
- **Controllers**: correct status codes and response shape for happy path and errors
- **Services**: business logic, edge cases, error propagation
- **Utils**: input/output transformations, pure functions
- **Middleware**: auth rejection, rate limit, validation errors
---
 
## 11. VERSION CONTROL
 
### Conventional Commits
Format: `type(scope): description`
 
| Type | When |
|------|------|
| `feat` | New feature or endpoint |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `refactor` | Code restructure, no behavior change |
| `test` | Adding or fixing tests |
| `chore` | Deps, config, tooling |
 
### Branch strategy
- `feature/descriptive-name` for new features
- `fix/descriptive-name` for bug fixes
- One logical change per commit
- Merge to `main` only when build and tests pass
---
 
## 12. PRE-DEPLOYMENT CHECKLIST
 
Run before every deploy:
 
```bash
# 1. Type check
npx tsc --noEmit
 
# 2. Lint
npm run lint
 
# 3. Tests
npm run test -- --coverage
 
# 4. Production build
npm run build
 
# 5. Dependency audit
npm audit --audit-level=moderate
```
 
Verify manually:
- [ ] `.env.prod` not committed to the repo
- [ ] No `console.log` in production code
- [ ] No hardcoded secrets or API keys
- [ ] All env vars documented in `.env.example`
- [ ] `/health` returns 200 after deploy before routing traffic
- [ ] Swagger docs up to date
- [ ] CHANGELOG.md updated with this release
---
 
## Quick Reference: Tech-Specific Notes
 
### Express / Node.js
- Use `express-async-errors` or wrap async handlers — unhandled promise rejections crash the process
- Register the error handler middleware last, always
- Use `helmet` for security headers out of the box
### NestJS
- Modules encapsulate domain logic — no cross-module direct imports
- Use Guards for auth, Interceptors for logging/transform, Pipes for validation
- Global exception filter replaces the centralized error handler
### FastAPI (Python)
- Pydantic models for all request/response validation — no manual validation
- Dependency injection for DB sessions and auth
- `HTTPException` for all error responses — never return raw dicts for errors
---
 
## When Generating Code
 
For every file generated, verify:
1. Layer is correct — controller doesn't query DB, service doesn't touch `req`/`res`
2. TypeScript strict types — no `any`, no implicit returns
3. One-line JSDoc per exported function
4. Input validated before the controller body runs
5. Error propagated to centralized handler, not swallowed
6. Logger used instead of `console.log`
7. New endpoint documented in Swagger
8. Remind user to add a test file for the new module
 
