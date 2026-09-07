# 🤖 Guía de IA para Ingenieros: Herramientas, MCPs y Prompts

Este repositorio recopila interfaces de diseño, utilidades de procesamiento, especificaciones del Model Context Protocol (MCP) y prompts avanzados para optimizar flujos de trabajo en ingeniería de software y desarrollo asistido por Inteligencia Artificial.

## Índice

- 🛠️ [Herramientas Útiles](#tools)
- 💪 [Skills Útiles](#skills)
- ⚙️ [Plugins Útiles](#plugins)
- 🔌 [MCPs Útiles](#mcp)
- ⚡ [Prompts Útiles](#prompts)

---

## 🛠️ Herramientas Útiles <a name="tools"></a>

Interfaces y utilidades optimizadas para el procesamiento de contexto, scraping estructurado y maquetación visual.

- 🛠️ [Hermes](https://hermes-agent.ai/) - Gestion de un Agente.
- 🛠️ [Google Stitch](https://stitch.withgoogle.com/) - Entorno colaborativo de Google enfocado en la estructuración, consistencia e inspección de patrones de diseño UI/UX.
- 🛠️ [Open CoDesign](https://opencoworkai.github.io/open-codesign/) - Framework de diseño abierto y colaborativo asistido por IA para la generación iterativa de componentes de interfaz.
- 🛠️ [Readdy AI](https://readdy.ai/) - Plataforma basada en IA para analizar tendencias estéticas y generar layouts web modernos de alta fidelidad.
- 🛠️ [Relume IO](https://www.relume.io/?r=0) - Generador de sitemaps y wireframes basados en componentes de Tailwind CSS y Webflow para acelerar la fase inicial de desarrollo frontend.
- 🛠️ [Google Mix Board](https://labs.google.com/mixboard/welcome) - Herramienta experimental de Google Labs orientada a la creación, mezcla y composición ágil de piezas publicitarias y multimedia.
- 🛠️ [MarkitDown](https://github.com/microsoft/markitdown) - Utilidad oficial de Microsoft para convertir archivos complejos (PDF, DOCX, XLSX) a texto Markdown limpio, reduciendo drásticamente el consumo de tokens.
- 🛠️ [Wacrawl](https://github.com/openclaw/wacrawl) - Herramienta de análisis estático y extracción estructurada de históricos de chat de WhatsApp para ingesta de datos.
- 🛠️ [MarkitDown Online](https://markitdown.online/l) - Interfaz web directa basada en la utilidad de conversión para transformar archivos planos y documentos de oficina en formato `.md`.
- 🛠️ [GitToSkill](https://www.gittoskill.com/) - Analizador automático de repositorios de GitHub que encapsula tu código base en formatos funcionales de "skills" asimilables por agentes de IA.
- 🛠️ [Google-Maps-Scraper](https://github.com/gosom/google-maps-scraper) - Aplicación de scraping optimizada en Go para la extracción masiva y estructurada de datos geográficos, reseñas y leads desde Google Maps.
- 🛠️ [reactbits.dev](https://reactbits.dev) - Aplicación para obtener prompts de secciones para apginas web.

---

## 💪 Skills Útiles <a name="skills"></a>

Capacidades técnicas avanzadas y abstracciones lógicas para expandir el alcance operativo de los modelos lingüísticos.

- 💪 [Remotion](https://www.remotion.dev/) - Framework que permite programar videos utilizando React, TypeScript y animaciones CSS, ideal para automatizar renders de video a través de prompts de IA (por ejemplo, Claude).
- 💪 [Hyperframes](https://github.com/heygen-com/hyperframes) - Motor de abstracción de código diseñado para la generación programática, estructuración secuencial y renderizado de frames multimedia complejos.
- 💪 [Napkin](https://github.com/blader/napkin) - Módulo de persistencia que almacena logs de excepciones, trazas de errores por proyecto y genera un grafo de memoria contextual para prevenir regresiones en agentes autónomos.

---

## ⚙️ Plugins Útiles <a name="plugins"></a>

Extensiones de infraestructura que conectan entornos de desarrollo con ecosistemas de modelos de lenguaje externos.

- ⚙️ [Codex Plugin Claude Code](https://github.com/openai/codex-plugin-cc) - Extensión de integración diseñada para interconectar arquitecturas de Codex con los entornos de ejecución en terminal de Claude Code.
- ⚙️ [Trail of Bits Skills](https://github.com/trailofbits/skills) - Conjunto de utilidades de análisis de seguridad automatizado en el pipeline de desarrollo para blindar el código del backend ante inyecciones de prompts o fallos lógicos.
- ⚙️ [Banana Claude](https://github.com/AgriciDaniel/banana-claude) - Capa de abstracción y ruteo diseñada para orquestar y alternar flujos de trabajo entre los modelos de Claude de Anthropic y la suite Gemini de Google.
- ⚙️ [Caveman](https://github.com/JuliusBrussee/caveman) - Compresor sintáctico y optimizador de AST (Abstract Syntax Tree) enfocado en podar redundancias en el código fuente para minimizar el consumo de tokens de contexto.

---

## 🔌 MCPs Útiles <a name="mcp"></a>

Protocolos estandarizados (Model Context Protocol) que integran de forma segura herramientas y servicios externos de terceros en las sesiones de chat de la IA.

- 🔌 [Apify MCP](https://apify.com/) - Conector nativo que dota a la IA de capacidades integradas de scraping web automatizado, extracción de datos web y análisis competitivo directo de la competencia.
- 🔌 [Google Stitch MCP](https://stitch.withgoogle.com/docs/mcp/setup) - Configuración del protocolo de enlace para permitir que los agentes de IA interactúen, lean e inyecten layouts directamente sobre el lienzo de Google Stitch.
- 🔌 [MCP-GSC](https://github.com/AminForou/mcp-gsc) - Integración con Google Search Console que habilita a los LLMs a consultar métricas de indexación, palabras clave, clics y errores de rastreo directamente por consola.

---

## ⚡ Prompts Útiles <a name="prompts"></a>

Instrucciones estructuradas y probadas bajo ingeniería de prompts para maximizar la calidad del output, la fidelidad técnica y el procesamiento visual de los modelos.

### 📝 Creación de Documentación para Proyectos

> Document this project: overview, architecture, setup, steps, key APIs with examples. output as DOCS.md

### 📝 Creación de Seguridad para Proyectos

> Revisa todos los inputs del usuario en mi app y añade validacion y sanitizacion estricta antes de procesarlos o guardarlos, uncluyendo forms, query params y body de requets
> Revisa todas mis queries y asegurate de que usen prepared statements, nunca concatenacion directa de strings del usuario.
> Configura Row Level Security en todas mis tablas para que cada usuarios solo pueda ver y modificar sus propios datos.
> Corre un audit de mis dependencias, identifica las vulnerables y actualizalas a versiones seguras sin romper la app.

### 💼 Optimización del Perfil en LinkedIn

> Actua como un consultor de crecimiento en LinkedIn de clase mundial que optimiza perfiles para visibilidad y oportunidades. Analiza mi perfil completo de LinkedIn e identifica debilidades en el titular, resumen, experiencia y posicionamiento. Porporciona mejoras especificas para aumentar el impacto del perfil. Perfil [url]() o como pdf generado directamente desde linkedIn [adjuntar pdf]().

### 📸 Efecto Fotográfico Específico

> Transforma esta foto como si hubiera sido tomada con un canon g7 max iii con flash.

### 💎 Optimización Extrema de Calidad Fotográfica (Versión Detallada)

> Transforma la imagen cargada de baja calidad y borrosa en una imagen con calidad cinematográfica y un nivel de detalle extremo. Mejora de imagen profesional de calidad ultra-premium. Preserva el 100% de la identidad original, la estructura facial, la expresión, la pose, la vestimenta, los accesorios, el fondo, el encuadre y la composición. NO alteres, rediseñes, reemplaces ni añadas nada. RECUPERACIÓN DE MICRODETALLES:
> * Rasgos faciales nítidos
> * Textura de piel natural
> * Poros visibles
> * Hebras de cabello realistas
> * Ojos cristalinos
> * Bordes limpios у refinados.
> Claridad de alto contraste, profundidad intensa e iluminación cinematográfica equilibrada. Realismo digno de un póster, con detalles dramáticos pero precisos. Salida en resolución 8K, calidad ProRes y nitidez de nivel de estudio. Solo texturas fotorrealistas. Solo mejoras fieles a la fuente original. Mantén todo exactamente igual; simplemente mejora la calidad.

### 🎬 Optimización Extrema de Calidad Fotográfica (Versión Compacta)

> Transforma tu imagen borrosa, de baja calidad y con mucho ruido visual en una imagen cinematográfica con detalles extremos. Conserva el 100% de la identidad original: estructura facial, expresión, pose, ropa, accesorios, fondo, encuadre y composición. NO alteres, redibujes, reemplaces ni añadas nada. RECUPERACIÓN DE MICRODETALLES: Rasgos faciales nítidos Textura de piel natural Poros visibles Mechones de cabello realistas Ojos cristalinos Bordes limpios у definidos Alto contraste, gran profundidad e iluminación cinematográfica equilibrada. Realismo digno de póster con detalles dramáticos y precisos. Salida en resolución 8K, calidad ProRes, nitidez de estudio. Solo texturas fotorrealistas. Mejoras fieles a la fuente original. Mantén todo igual, solo mejora la calidad.

### 🛠️ Restauración Estética de Imágenes Conservando Identidad

> Recupera esta foto sin hacer ningún cambio a la persona, rostro o vestimenta. Quiero la misma foto exacta pero restaurada, quiero el fondo actualizado pero no cambiado, texturas de piel suaves y realistas. y una iluminacion natural. La vestimenta y fondo deben actualizarse de manera estética y limpia. mientras se conserva la autenticidad de la posa y las expresiones naturales.

### 🎨 Análisis Editorial de Colorimetría y Visajismo Personalizado

> Usando este retrato, crea un análisis visual premium de imagen personal. Empieza con un diagrama de colorimetría mostrando las estaciones (Primavera, Verano, Otoño, Invierno) y destaca cuál encaja mejor con el sujeto. Después muestra comparativas visuales lado a lado de colores favorecedores vs colores que apagan, incluyendo ropa, neutros, colores acento y metales (oro vs plata). Añade diagnóstico de subtono de piel y referencias visuales estilo asesoría de imagen de lujo. Haz que parezca una infografía editorial moderna y muy compartible. Prioriza lo visual sobre el texto, usa solo etiquetas cortas, sin párrafos, estética limpia y formato vertical 9:16. Si vas a utilizar fotos de modelos o fotos de prendas de ropa, utiliza mi cara.

### 📐 Auditoría de Estructura y Proporciones Faciales de Gama Alta

> Crea un informe de belleza facial limpio, minimalista y de alta gama basado en esta foto. Usa un diseño en blanco y negro, con líneas finas, tarjetas con bordes redondeados y una estética de lujo. Incluye un dibujo simple del contorno del rostro, un análisis honesto del atractivo (simetría, proporciones, estructura ósea, piel, etc.), puntuaciones claras, puntos fuertes, áreas de mejora y recomendaciones prácticas de grooming/estilo. Mantén un enfoque basado en datos, visualmente refinado y sin ser excesivamente halagador.

### 🤖 Generación de Repositorio de Contexto Técnico para Agentes de IA (Ficheros de Contexto de Alta Densidad)

> Lee mi proyecto entero (estructura de carpetas, dependencias, código, tests, README y commits recientes) y genera 6 documentos de contexto en docs/contexto/, uno por archivo:
> 
> 1. `arquitectura.md` — stack, mapa de carpetas, flujo de datos, qué NO existe.
> 2. `convenciones.md` — estilo, naming, patrones que usamos y los prohibidos, tests, commits.
> 3. `decisiones.md` — decisiones técnicas que detectes en el código/commits, con su porqué y lo descartado.
> 4. `glosario.md` — términos del dominio, entidades principales y siglas internas.
> 5. `flujo-de-trabajo.md` — pasos para hacer un cambio, checklist de "terminado" y deploy.
> 6. `errores-conocidos.md` — gotchas que se intuyan del código, tests y comentarios.
> 
> Reglas:
> - Básate SOLO en lo que veas en el repo. No inventes.
> - Donde no haya información suficiente, deja un hueco marcado [PENDIENTE: ...] en vez de rellenar a ciegas.
> - Sé concreto y breve: cada doc se lee en menos de 2 minutos.

---

## 🔗 Referencias de Links e Imágenes <a name="references"></a>

### Fuentes de Herramientas y Ecosistema
* [Google Stitch UI & Setup](https://stitch.withgoogle.com/)
* [Open CoDesign Platform](https://opencoworkai.github.io/open-codesign/)
* [Readdy AI Web Analytics](https://readdy.ai/)
* [Relume Design Wireframing](https://www.relume.io/?r=0)
* [Google Mix Board Interactive](https://labs.google.com/mixboard/welcome)
* [Microsoft MarkitDown GitHub](https://github.com/microsoft/markitdown)
* [Wacrawl Analysis Engine](https://github.com/openclaw/wacrawl)
* [MarkitDown Online Tool](https://markitdown.online/l)
* [GitToSkill Service](https://www.gittoskill.com/)
* [Google Maps Scraper Engine](https://github.com/gosom/google-maps-scraper)

### Recursos de Capacidades y Extensiones
* [Remotion Video Programming Framework](https://www.remotion.dev/)
* [Hyperframes Layout Engine](https://github.com/heygen-com/hyperframes)
* [Napkin State Logger](https://github.com/blader/napkin)
* [OpenAI Codex Plugin Claude Code](https://github.com/openai/codex-plugin-cc)
* [Trail of Bits Cybersecurity Skills](https://github.com/trailofbits/skills)
* [Banana Claude Core Router](https://github.com/AgriciDaniel/banana-claude)
* [Caveman Token Minimizer](https://github.com/JuliusBrussee/caveman)

### Integraciones de Protocolo MCP
* [Apify Web Scraping Solutions](https://apify.com/)
* [Google Stitch MCP Documentation](https://stitch.withgoogle.com/docs/mcp/setup)
* [Google Search Console MCP Protocol](https://github.com/AminForou/mcp-gsc)

### Documentación e Inspiración de Contexto
* [Plantillas de Contexto Avanzado (Pablo In Public)](https://pabloinpublic.com/recursos/plantillas-contexto-claude)
