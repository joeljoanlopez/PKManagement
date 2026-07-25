# Cap 05 — API: router y arquitectura en capas

## Objetivo
Construir a mano un router y una estructura en capas que convierta peticiones HTTP en respuestas JSON, con CORS resuelto.

## Por qué
Esto es el corazón de lo que Laravel hacía por ti (`routes/api.php`, controladores, middleware). Construirlo te da el modelo mental completo de un framework web.

> **Conexión con hexagonal ([cap 04b](04b-arquitectura-hexagonal.md))**: todo lo de este capítulo —router, controladores, middleware— es tu **adaptador de entrada** (driving adapter). Traduce HTTP a llamadas a casos de uso y traduce las respuestas/errores del dominio a JSON + códigos HTTP. El controlador NO contiene lógica de negocio ni SQL: invoca un caso de uso y formatea la respuesta.

## Conceptos a dominar
- **Front controller**: todas las peticiones entran por un único `index.php` (nginx reescribe hacia él). Igual que `public/index.php` de Laravel.
- **Router**: mapear `MÉTODO + ruta` → un manejador. Soporte de parámetros dinámicos en la ruta (`/subscribers/{id}`).
- **Capas y responsabilidades** (encajan con hexagonal, [cap 04b](04b-arquitectura-hexagonal.md)):
  - *Controller* (adaptador de entrada): recibe la petición, valida el formato de entrada, invoca un caso de uso, formatea la respuesta. Traduce excepciones de negocio a códigos HTTP.
  - *Caso de uso / Servicio de aplicación*: orquesta la lógica de negocio. Vive en la capa de aplicación, no conoce HTTP.
  - *Repositorio*: interfaz en el dominio, implementación PDO en infraestructura. El controlador no ve SQL.
- **Middleware manual**: funciones que se ejecutan antes del controlador (CORS, autenticación, parseo de JSON). Entiende el patrón de "pipeline".
- **CORS a fondo**: qué es una petición *preflight* (`OPTIONS`), qué headers debe devolver el servidor (`Access-Control-Allow-Origin/Methods/Headers`), y por qué el navegador la exige.
- **Validación de entrada**: comprobar campos requeridos, tipos, formatos; devolver 422 con detalles.
- **Formato de respuesta consistente**: estructura fija para éxitos y errores.
- Códigos HTTP correctos por caso (200, 201, 204, 400, 401, 403, 404, 422, 500).

## Preguntas de autoevaluación
1. ¿Por qué conviene un único punto de entrada en vez de un `.php` por endpoint?
2. ¿Cómo extraes el `{id}` de una ruta como `/subscribers/42`?
3. ¿Qué es una petición preflight y cuándo la dispara el navegador?
4. ¿En qué capa debe vivir el SQL y por qué el controlador no debería tocarlo?
5. ¿Qué diferencia hay entre responder 401, 403 y 404? Da un caso de cada uno en tu app.

## Ejercicios
- [ ] Implementa un router que registre rutas (método + patrón) y despache a un manejador.
- [ ] Haz que el router extraiga parámetros dinámicos de la URL.
- [ ] Implementa el mecanismo de middleware (al menos: CORS y parseo de JSON).
- [ ] Responde correctamente a las peticiones `OPTIONS` (preflight) con 204.
- [ ] Crea la estructura de carpetas: Controllers, Repositories, (Services), Middleware.
- [ ] Define tu formato estándar de respuesta de éxito y de error, y aplícalo en todos lados.
- [ ] Crea el primer endpoint real: `GET /api/subscribers` (Controller → Repository → JSON). Pruébalo con `curl`.

## Criterio de dominio
Puedes añadir un nuevo endpoint (ruta → controlador → repositorio → respuesta) sin duplicar lógica, y una petición desde otro origen funciona sin errores de CORS.

## Checklist
- [ ] Front controller único
- [ ] Router con parámetros dinámicos
- [ ] Pipeline de middleware
- [ ] CORS + preflight resueltos
- [ ] Separación Controller / Repository
- [ ] Formato de respuesta consistente
- [ ] GET /api/subscribers probado con curl

## Recursos
- MDN — [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) (lectura obligatoria y detallada).
- Lee `public/index.php` de un proyecto Laravel para ver el patrón front controller real.
- MDN — [métodos HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) y [códigos de estado](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status).

## Enlaces internos
- Anterior: [cap 04b — Arquitectura hexagonal](04b-arquitectura-hexagonal.md) · Siguiente: [cap 06 — Autenticación](06-autenticacion.md)
- Aplica: [cap 04b — Hexagonal (adaptador de entrada)](04b-arquitectura-hexagonal.md). Se prueba en: [cap 09b — Tests funcionales](09b-testing-y-calidad.md). Se consume desde: [cap 08 — React consumo de API](08-react-consumo-api.md)
- Índice: [00 — Índice](00-INDICE.md)
