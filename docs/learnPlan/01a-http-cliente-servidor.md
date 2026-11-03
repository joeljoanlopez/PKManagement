# Cap 01a — HTTP y el modelo cliente-servidor

> Parte de [cap 01 — Fundamentos y arquitectura](01-fundamentos-y-arquitectura.md) · Siguiente: [01b — Arquitectura del sistema](01b-arquitectura-sistema.md)

## Objetivo
Saber leer y explicar una petición y una respuesta HTTP completas, línea a línea, y razonar qué método y código de estado corresponde a cada situación.

## Por qué
En Laravel, `Route::post`, `$request->input()` y `return response()->json(..., 201)` te esconden el protocolo. Sin framework vas a leer la petición en crudo (`$_SERVER`, `php://input`) y a escribir la respuesta a mano (`http_response_code`, `header`). Si no entiendes HTTP, todo lo demás es magia.

## Conceptos a dominar
- **Anatomía de una petición**: línea de petición (método, ruta, versión), cabeceras, línea en blanco, cuerpo.
- **Anatomía de una respuesta**: línea de estado (código + frase), cabeceras, cuerpo.
- **Cabeceras clave**: `Host`, `Content-Type`, `Accept`, `Content-Length`, `Authorization`, `Location`.
- **Métodos**: GET, POST, PUT, PATCH, DELETE. Propiedades **seguro** (*safe*) e **idempotente**.
- **Códigos de estado por familias** (1xx–5xx) y los que vas a usar de verdad: 200, 201, 204, 400, 401, 403, 404, 409, 422, 500.
- **HTTP no tiene estado** (*stateless*): cada petición lleva todo lo necesario. Qué implica eso para la autenticación (preparación para el [cap 06](06-autenticacion.md)).
- **JSON** como formato de intercambio: tipos que admite y cómo se declara (`Content-Type: application/json`).

## Preguntas de autoevaluación
1. ¿Qué separa las cabeceras del cuerpo en un mensaje HTTP?
2. ¿Qué diferencia hay entre un método *seguro* y uno *idempotente*? Pon un ejemplo de cada combinación posible.
3. ¿Por qué POST no es idempotente y PUT sí? ¿Y PATCH?
4. ¿Cuándo usarías 401 y cuándo 403? ¿Y 400 frente a 422?
5. Si HTTP no tiene estado, ¿cómo "sabe" el servidor que ya has hecho login?
6. ¿Qué pasa si envías JSON en el cuerpo pero con `Content-Type: text/plain`?

## Ejercicios
Escribe tus respuestas en `docs/notes/01a-http.md` (ver [cómo organizar las notas](../notes/README.md)). Haz commit al cerrar cada ejercicio.

### 1. Disecciona una petición real
1. Lee la sección *"HTTP messages"* del Overview de MDN.
2. Ejecuta `curl -v https://api.github.com/users/octocat` y guarda la salida.
3. Identifica en la salida: la línea de petición, cada cabecera de la petición, la línea de estado, tres cabeceras de la respuesta y el cuerpo. Explica qué significan los prefijos `>`, `<` y `*` de curl (consulta `man curl`, opción `-v`).
4. Repite la petición con un usuario que no exista. ¿Qué cambia?

### 2. Envía datos
1. Investiga en el manual de curl las opciones `-X`, `-H`, `-d` y `-i`.
2. Haz un POST con cuerpo JSON a `https://postman-echo.com/post`, un servicio que te devuelve lo que le envías. Comprueba en la respuesta que el servidor ha recibido tu cuerpo y tus cabeceras.
3. Repite el POST sin la cabecera `Content-Type` y compara la respuesta.

### 3. Tabla de métodos
Construye **de memoria** (y luego compárala con MDN/RFC 9110) una tabla con las columnas: método · ¿seguro? · ¿idempotente? · ¿lleva cuerpo normalmente? · para qué lo usarías en el parking.

### 4. Escenarios → código de estado
Para cada escenario, escribe el código de estado y **por qué** ese y no otro parecido:
1. Listar abonados correctamente.
2. Crear un abonado correctamente.
3. Borrar un vehículo correctamente (sin cuerpo en la respuesta).
4. Pedir un abonado con un id que no existe.
5. Crear un abonado con un documento (DNI) que ya existe.
6. Crear un abonado sin el campo obligatorio `nombre`.
7. Enviar un JSON mal formado (falta una llave).
8. Acceder a la API sin token.
9. Un operario sin permisos intenta borrar un pago.
10. Asignar una plaza que ya está ocupada.
11. La base de datos está caída.

## Criterio de dominio
Puedes leer la salida de `curl -v` sin ayuda y justificar el código de estado de cada escenario del ejercicio 4 frente a la alternativa más parecida.

## Checklist
- [ ] Petición real diseccionada con `curl -v`
- [ ] POST con JSON enviado y verificado
- [ ] Tabla de métodos (seguro / idempotente)
- [ ] Escenarios → códigos de estado justificados

## Recursos
- MDN — [Overview of HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview) (lee especialmente "HTTP messages").
- MDN — [HTTP request methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods), [Safe](https://developer.mozilla.org/en-US/docs/Glossary/Safe/HTTP), [Idempotent](https://developer.mozilla.org/en-US/docs/Glossary/Idempotent).
- MDN — [HTTP response status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status), [HTTP headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers).
- [RFC 9110 — HTTP Semantics](https://www.rfc-editor.org/rfc/rfc9110): la fuente normativa. Consúltala para resolver dudas, no para leerla entera (secciones 9 *Methods* y 15 *Status Codes*).
- [RFC 8259 — JSON](https://www.rfc-editor.org/rfc/rfc8259).
- [Manual de curl](https://curl.se/docs/manpage.html).
