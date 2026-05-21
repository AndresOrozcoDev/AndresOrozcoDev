# REST API Rules — Full Reference
 
Read this file when designing a new endpoint, reviewing an existing one, or auditing an API.
Covers all 50 rules mapped to actionable standards.
 
---
 
## NAMING AND STRUCTURE
 
**1. HTTP verbs — use correctly**
- GET: read, no side effects
- POST: create or trigger a non-idempotent action
- PUT: full replace of a resource
- PATCH: partial update
- DELETE: remove
**2 & 3. Resources as plural nouns, coherent and predictable**
```
GET    /api/v1/users
GET    /api/v1/users/:id
POST   /api/v1/users
PUT    /api/v1/users/:id
PATCH  /api/v1/users/:id
DELETE /api/v1/users/:id
```
 
**4. Clean, hierarchical URLs**
- Kebab-case: `/user-profiles`
- Max 2 nesting levels: `/users/:id/orders` — not deeper
- Complex operations via sub-resources: `POST /orders/:id/cancel`
**36. No RPC-like endpoints**
`POST /api/v1/emails` not `/api/v1/sendEmail`
 
**38. URI vs query params**
- Path param: identifies a resource (`/users/:id`)
- Query param: filters, sorts, paginates (`?status=active&sortBy=name`)
- Never put verbs or actions in the path
**39. Manage relations via links, not deep nesting**
Return `{ id, userId, href: '/api/v1/users/123' }` instead of embedding full user object
when the related resource is large or frequently changed
 
**40. GET has zero side effects**
Never mutate data, trigger emails, or change state on a GET request
 
---
 
## RESPONSE FORMAT
 
**13. Consistent envelope**
```ts
// Single resource
{ success: true, data: T }
 
// Collection
{ success: true, data: T[], meta: { page, limit, total, totalPages } }
 
// Error
{ success: false, error: { code: string, message: string, details?: unknown } }
```
 
**15. JSON as default**
`Content-Type: application/json` always set in responses
 
**16. Never expose internal models**
- Separate Response DTOs from DB entities
- Strip: `passwordHash`, `__v`, `deletedAt`, raw DB IDs when a public UUID exists
**27. Standardized metadata fields**
```ts
meta: {
  page: number,
  limit: number,
  total: number,
  totalPages: number,
  links?: { next?: string, prev?: string }
}
```
 
**47. Naming consistency**
- Pick camelCase or snake_case for JSON fields — never mix in the same API
- Boolean fields: affirmative (`isActive` not `disabled`, `hasAccess` not `access`)
**48. Avoid ambiguous booleans**
If a field could evolve beyond true/false, use a status string or enum from day one:
`status: 'active' | 'suspended' | 'pending'` instead of `isActive: boolean`
 
---
 
## PAGINATION, FILTERING, SORTING
 
**8. Consistent pagination**
```
GET /api/v1/users?page=1&limit=20
```
- Default limit: 20. Hard max: 100. Enforce server-side.
- Always return `meta.total`
- Never return unbounded collections
**9. Standard filtering and sorting**
```
GET /api/v1/users?status=active&sortBy=createdAt&order=desc
```
- `sortBy` accepts only a whitelist of column names — never pass raw user input to DB query
- Document allowed filter fields in Swagger
**34. Sparse fieldsets (optional, only with measured need)**
```
GET /api/v1/users?fields=id,email,name
```
Implement only if there is a real performance or bandwidth constraint.
 
---
 
## HTTP AND PROTOCOL
 
**5 & 25. HTTP status codes**
| Code | When |
|------|------|
| 200 | Success with body |
| 201 | Created |
| 204 | No body |
| 304 | Not modified (ETag match) |
| 400 | Validation / bad request |
| 401 | Not authenticated |
| 403 | Not authorized |
| 404 | Not found |
| 409 | Conflict |
| 422 | Unprocessable entity |
| 429 | Rate limit exceeded |
| 500 | Server error |
| 503 | Temporarily unavailable |
 
**14. Content negotiation**
- `Accept` header respected when the API supports multiple formats
- Always return `Content-Type` in response
**19. Idempotency**
- GET, PUT, DELETE: idempotent by design
- POST: not idempotent by default — for critical operations (payments, emails), use:
  `Idempotency-Key: <uuid>` header — store key + result and replay on duplicate
- PATCH: design to be idempotent when possible
**29. ETags and HTTP caching**
- Return `ETag` on GET responses for infrequently-changing resources
- Support `If-None-Match` → 304 when ETag matches
- `Cache-Control: no-store` for sensitive/private data
- `Cache-Control: max-age=3600` for public cacheable resources
**30. Compression**
- Enable gzip/brotli at server or reverse proxy level
- Never compress already-compressed formats (images, PDFs, zip)
**22. Timeouts and payload limits**
- Max request body size configured at server level (default 1MB, increase only when justified)
- Long-running operations → `202 Accepted` + job ID, poll via `GET /jobs/:id`
- Document timeout behavior in Swagger and DOCS.md
---
 
## DOCUMENTATION
 
**6. OpenAPI / Swagger**
- Auto-generated, never written manually
- Served at `/api-docs`, always in sync with code
- Every endpoint documents: params, body, all response shapes (including errors), auth, limits
- At least one request/response example per endpoint
**10. Document parameters and formats**
- Allowed values for filter params
- Date format: ISO 8601 (`2024-03-15T12:00:00Z`)
- Enum values listed explicitly
**26. Usage examples**
- At least one happy-path example per endpoint in Swagger
- Error examples for the most common failure cases (400, 401, 404)
**42. Expose limits and quotas in documentation**
- Rate limits, max payload size, max bulk items documented in Swagger and README
- Exposed in response headers: `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset`
---
 
## VERSIONING AND COMPATIBILITY
 
**7. API versioning**
- URL path: `/api/v1/`, `/api/v2/` — preferred for public APIs
- Header alternative: `Accept: application/vnd.api+json;version=2`
- Never modify behavior of an existing version
**20. Backwards compatibility**
- Never remove or rename fields in an existing version
- Never change the type of an existing field
- Add new fields; don't replace old ones
- Breaking changes = new major version
**21. Contract testing**
- Test that response shapes match the documented contract
- Use contract tests (e.g. Pact) for APIs consumed by multiple clients
- Run contract tests in CI — a failed contract test blocks deployment
**23. Data versioning and migrations**
- DB migrations are reversible (`up` + `down`)
- API version and DB schema version can evolve independently
- Document migration path in CHANGELOG when changing data shape
**43. Deprecation policy**
- Mark in Swagger: `deprecated: true`
- Return headers on deprecated routes:
  `Deprecation: true` / `Sunset: 2025-12-31T00:00:00Z`
- Minimum 3-month notice. Document migration path.
- Remove only after sunset date passes
**46. Metadata for versioning and changes**
- CHANGELOG.md updated with every release
- Swagger `info.version` matches the deployed API version
---
 
## SECURITY
 
**17. Auth and authorization**
- JWT with short expiry + refresh token rotation, or API Key in `Authorization` / `X-API-Key`
- Auth checked in middleware, never inside the controller
- 401 = not authenticated. 403 = authenticated but lacks permission.
**18. Rate limiting**
- Applied per IP on all public endpoints
- Applied per user/token on authenticated endpoints
- `429` returned when limit exceeded
- Limits documented and returned in headers
**28. Input validation and sanitization**
- All body, params, and query validated before controller runs (Zod, Joi, class-validator)
- Parameterized queries only — never string concatenation in DB layer
- Strip unexpected fields from request body (whitelist, not blacklist)
---
 
## OPERATIONAL
 
**24. Specific endpoints for complex operations**
When an operation doesn't map cleanly to CRUD, create an explicit sub-resource:
`POST /api/v1/orders/:id/cancel` — not `PATCH /api/v1/orders/:id` with `{ action: 'cancel' }`
 
**32 & 33. Request logging and Correlation IDs**
- Every request logged: timestamp, method, path, status, duration, correlationId
- `X-Request-ID` generated if not received
- `X-Correlation-ID` propagated across service boundaries
**44. Bulk operations**
```
POST /api/v1/users/bulk
Body: { items: User[] }  // max 100 items, enforced server-side
```
Return partial success:
```ts
{ success: true, data: { created: User[], failed: [{ item, error }] } }
```
 
**45. Internationalization**
- All dates/times: ISO 8601 UTC (`2024-03-15T12:00:00Z`)
- `Accept-Language` header accepted for localized error messages
- Never return locale-formatted numbers or dates — the client formats
**49. Monitoring and SLIs**
- Track per endpoint: request count, error rate, p50/p95/p99 latency
- Define SLIs before go-live: e.g. "99% of requests complete under 500ms"
- Expose `/metrics` (Prometheus) or push to observability platform
---
 
## DESIGN DECISIONS
 
**11. HATEOAS**
Apply only when clients are hypermedia-driven. Minimum useful form:
```ts
{ data: User, links: { self: '/users/123', orders: '/users/123/orders' } }
```
Do not add speculatively — link objects without consuming clients are dead weight.
 
**41. Client timeout and retry policies**
- Document recommended client timeout in API docs
- Idempotent endpoints (`GET`, `PUT`, `DELETE`) are safe to retry
- POST endpoints: document whether they are safe to retry (idempotency key required if not)
**50. When to consider alternatives to REST**
| Protocol | Use when |
|----------|----------|
| REST | Default. Public APIs, CRUD-heavy services, multiple client types |
| GraphQL | Multiple consumers with different field needs, complex nested queries |
| gRPC | Internal service-to-service, high throughput, schema-first contracts |
| WebSockets | Real-time bidirectional: chat, live dashboards, event streams |
 
REST is the default. Switch only with a specific, documented technical reason.
 
