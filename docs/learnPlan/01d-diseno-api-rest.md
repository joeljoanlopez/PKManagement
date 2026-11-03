# Cap 01d — Diseño de la API REST (el contrato)

> Parte de [cap 01 — Fundamentos y arquitectura](01-fundamentos-y-arquitectura.md) · Anterior: [01c — Orígenes y CORS](01c-origenes-cors.md) · Siguiente: [01e — MVP, monorepo y cierre](01e-mvp-monorepo-cierre.md)

## Objetivo
Escribir el **contrato de la API** del MVP: recursos, rutas, métodos, códigos de estado, formato de las peticiones y respuestas, y formato de errores. Es el único acoplamiento entre frontend y backend, así que es el documento más importante del proyecto.

## Por qué
En Laravel, `Route::apiResource` te genera las rutas y no piensas en ellas. Aquí las diseñas tú, y el frontend (caps 07-08) y el backend (caps 05-09) se construirán contra este documento. Un contrato mal pensado se paga en cada capítulo posterior.

## Conceptos a dominar
- **Recurso**: un sustantivo del dominio, no una acción. Colección (`/abonados`) vs elemento (`/abonados/{id}`).
- **Convenciones de rutas**: plural, minúsculas, sin verbos, prefijo `/api`.
- **Recursos anidados**: cuándo `/abonados/{id}/vehiculos` y cuándo `/vehiculos/{id}`. Qué criterio usas.
- **Acciones que no son CRUD** (suspender un abonado, cancelar una suscripción): modelarlas como cambio de estado (`PATCH`) o como subrecurso. Ventajas de cada opción.
- **Query params** para filtrar, buscar y paginar (`?q=`, `?page=`, `?estado=`). Qué metadatos de paginación devuelve la respuesta.
- **PUT vs PATCH**: sustitución completa vs parcial.
- **Respuesta de creación**: `201 Created` + cabecera `Location` + recurso creado en el cuerpo.
- **Formato de error uniforme**: una sola forma para todos los errores, incluidos los de validación por campo. Estándar de referencia: *Problem Details* (RFC 9457).
- **Versionado** (`/api/v1`): qué resuelve y si lo necesitas en un MVP.

## Preguntas de autoevaluación
1. ¿Por qué `/abonados/{id}/suspender` no es REST "puro"? ¿Qué alternativas tienes?
2. ¿Qué criterio usas para anidar un recurso bajo otro?
3. ¿Qué devuelves al crear un recurso, además del código 201?
4. ¿Cómo sabe el frontend que hay más páginas de resultados?
5. ¿Por qué conviene que todos los errores tengan el mismo formato? ¿Qué gana el frontend?
6. ¿Qué operaciones de tu API son idempotentes? ¿Alguna debería serlo y por diseño no lo es?

## Ejercicios
Entregable: `docs/api/contract.md`, escrito por ti. Las respuestas a la autoevaluación y tus dudas van en `docs/notes/01d-api.md`.

### 1. Recursos y relaciones
1. Lista los recursos del dominio (operarios/sesión, abonados, vehículos, plazas, suscripciones, pagos, dashboard).
2. Para cada uno, escribe sus atributos principales (sin tipos SQL todavía; eso es el [cap 04](04-mysql-pdo-modelado.md)) y sus estados, si los tiene.
3. Escribe las relaciones entre ellos con cardinalidad ("un abonado tiene N vehículos", etc.).
4. Decide cuáles anidas y cuáles no, y escribe el criterio que has seguido.

### 2. Tabla de endpoints
Para cada recurso, una tabla con: **acción de usuario → método + ruta → cuerpo de entrada → código(s) de éxito → códigos de error posibles**.
- Cubre las 7 features del MVP del README, incluido el login y la detección de impagos.
- Para la búsqueda y paginación de abonados, define los query params y la forma de la respuesta paginada.
- Para las acciones no-CRUD (suspender, cancelar, marcar como pagado…), elige un estilo y aplícalo de forma consistente.

### 3. Ejemplos de mensajes
Para **tres** endpoints (uno de listado paginado, uno de creación y uno con regla de negocio, como asignar una plaza), escribe la petición y la respuesta completas de ejemplo: método, ruta, cabeceras relevantes, cuerpo JSON y código de estado.

### 4. Formato de errores
1. Lee el RFC 9457 (secciones 3 y 4 bastan) y decide si lo adoptas tal cual, una versión simplificada o tu propio formato.
2. Define el formato y escribe tres ejemplos: error de validación con varios campos, 404, y 409 por la regla de "plaza ocupada".

### 5. Revisión cruzada
Repasa tu tabla con la tabla de escenarios del [01a](01a-http-cliente-servidor.md) (ejercicio 4): ¿encajan los códigos? Pide revisión al instructor antes de dar el contrato por cerrado.

## Criterio de dominio
Con solo tu `contract.md`, otra persona podría construir el frontend sin preguntarte nada. Y puedes justificar cada decisión no obvia: anidamiento, acciones no-CRUD y formato de error.

## Checklist
- [ ] Recursos, atributos, estados y relaciones
- [ ] Tabla de endpoints completa del MVP
- [ ] Ejemplos de petición/respuesta de 3 endpoints
- [ ] Formato de errores definido con ejemplos
- [ ] `docs/api/contract.md` revisado

## Recursos
- Roy Fielding — [Disertación, cap. 5: REST](https://ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm) (la fuente original; lee 5.1 y 5.2.1).
- [RFC 9110 — HTTP Semantics](https://www.rfc-editor.org/rfc/rfc9110): secciones 9.3 (métodos) y 10.2.2 (`Location`).
- [RFC 5789 — PATCH](https://www.rfc-editor.org/rfc/rfc5789).
- [RFC 9457 — Problem Details for HTTP APIs](https://www.rfc-editor.org/rfc/rfc9457).
- Laravel — [Resource controllers](https://laravel.com/docs/controllers#resource-controllers): compara la tabla de rutas que genera `apiResource` con la tuya.
