# Cap 01c — Orígenes, Same-Origin Policy y CORS

> Parte de [cap 01 — Fundamentos y arquitectura](01-fundamentos-y-arquitectura.md) · Anterior: [01b — Arquitectura del sistema](01b-arquitectura-sistema.md) · Siguiente: [01d — Diseño de la API REST](01d-diseno-api-rest.md)

## Objetivo
Predecir, antes de escribir código, qué peticiones de tu frontend bloqueará el navegador y por qué. La implementación llega en el [cap 05](05-api-router-capas.md); aquí solo se entiende el mecanismo.

## Por qué
En Laravel con Blade, la página y el formulario salían del mismo servidor: nunca tuviste un problema de CORS. Con React en `:5173` y la API en `:8080` lo tendrás el primer día. Si no entiendes el mecanismo, acabarás poniendo `Access-Control-Allow-Origin: *` sin saber qué implica.

## Conceptos a dominar
- **Origen** = esquema + host + puerto.
- **Same-Origin Policy**: quién la aplica (el navegador, no el servidor) y qué protege.
- **CORS**: el servidor *relaja* esa política con cabeceras (`Access-Control-Allow-Origin`, `-Methods`, `-Headers`).
- **Peticiones simples vs con preflight** (`OPTIONS`): qué hace que una petición necesite preflight.
- **Por qué curl/Postman no tienen CORS**.
- **Cómo desaparece el problema en producción** si frontend y API se sirven desde el mismo origen (proxy `/api`), y qué alternativa existe en desarrollo (proxy de Vite).

## Preguntas de autoevaluación
1. ¿Por qué `http://localhost:5173` y `http://localhost:8080` son orígenes distintos?
2. ¿Quién bloquea la respuesta: el servidor o el navegador? ¿Llega la petición al servidor?
3. ¿Por qué una petición con `curl` a tu API nunca da error de CORS?
4. ¿Qué es una petición preflight, con qué método se hace y qué espera recibir?
5. ¿Qué riesgo tiene responder `Access-Control-Allow-Origin: *` en una API con autenticación?

## Ejercicios
Responde en `docs/notes/01c-cors.md`.

### 1. ¿Mismo origen?
Para cada par, di si es el mismo origen y, si no, qué componente cambia:
1. `http://localhost:5173` vs `http://localhost:8080`
2. `http://localhost:8080/api/abonados` vs `http://localhost:8080/api/plazas`
3. `http://localhost:8080` vs `https://localhost:8080`
4. `http://localhost:8080` vs `http://127.0.0.1:8080`
5. `https://parking.ejemplo.com` vs `https://api.parking.ejemplo.com`
6. `https://parking.ejemplo.com` vs `https://parking.ejemplo.com:443`

### 2. Predice el preflight
Con la sección *"Simple requests"* de MDN delante, decide para cada petición de TU app si provocará preflight y qué condición lo dispara:
1. `GET /api/plazas` sin cabeceras extra.
2. `POST /api/abonados` con cuerpo JSON.
3. `GET /api/abonados` con cabecera `Authorization: Bearer ...`.
4. `DELETE /api/vehiculos/3`.

### 3. Míralo con tus ojos
1. Abre las DevTools de tu navegador en cualquier web y, desde la consola, haz un `fetch` a una URL de otro origen (por ejemplo, `https://example.com`).
2. Lee el error de la consola y busca la petición en la pestaña *Network*. ¿Llegó la petición? ¿Hubo respuesta?
3. Repite la misma URL con `curl` y explica la diferencia.

### 4. Decide tu estrategia
Responde: en desarrollo, ¿usarás cabeceras CORS en PHP o el proxy de Vite? ¿Y en producción, necesitarás CORS? Justifícalo con tu diagrama del 01b.

## Criterio de dominio
Puedes predecir si una petición concreta de tu frontend va a fallar por CORS, por qué, y qué cabecera o configuración lo resuelve, sin probarlo antes.

## Checklist
- [ ] Ejercicio de orígenes resuelto
- [ ] Predicción de preflight para mis peticiones
- [ ] Error CORS observado en DevTools
- [ ] Estrategia CORS dev/prod decidida

## Recursos
- MDN — [Same-origin policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy).
- MDN — [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) (lee *Simple requests* y *Preflighted requests*).
- MDN — [Origin (glosario)](https://developer.mozilla.org/en-US/docs/Glossary/Origin).
- Vite — [`server.proxy`](https://vite.dev/config/server-options#server-proxy).
