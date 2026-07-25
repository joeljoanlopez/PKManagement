# Cap 06 — Autenticación

## Objetivo
Implementar login de operarios y proteger endpoints, sin Sanctum ni Passport: hashing de contraseñas y tokens (JWT).

## Por qué
Laravel te daba auth "gratis". Aquí entiendes qué es un token, cómo se firma y valida, y cómo se almacenan contraseñas de forma segura. Conocimiento que aplica a cualquier stack.

## Conceptos a dominar
- **Hashing de contraseñas**: `password_hash` (bcrypt/argon2) y `password_verify`. Por qué NUNCA se guardan contraseñas en texto ni con hash reversible/MD5.
- **Salt** y por qué `password_hash` ya lo gestiona.
- **Autenticación vs autorización**: quién eres vs qué puedes hacer.
- **JWT (JSON Web Token)**: estructura (header.payload.signature), firma, expiración (`exp`), qué poner y qué NO poner en el payload (nunca datos sensibles).
- Flujo: login → servidor emite token firmado → cliente lo guarda → lo envía en `Authorization: Bearer <token>` → middleware lo valida en cada petición protegida.
- **Middleware de autenticación**: valida el token, rechaza con 401 si falta o es inválido, inyecta el usuario en la petición.
- Consideraciones de seguridad: expiración corta, dónde guarda el token el frontend (implicaciones), qué hacer al cerrar sesión.
- (Alternativa a conocer: sesiones con cookies. Compara ambos enfoques y decide con criterio).

## Preguntas de autoevaluación
1. ¿Por qué `password_hash` produce un hash distinto cada vez para la misma contraseña, y cómo puede entonces `password_verify` validarla?
2. ¿Qué contiene exactamente un JWT y qué impide que un atacante lo modifique?
3. ¿Por qué NO debes poner la contraseña ni datos sensibles en el payload del JWT?
4. ¿Qué código HTTP devuelves si el token falta, y cuál si el usuario no tiene permiso para esa acción?
5. ¿Qué riesgos tiene guardar el token en `localStorage` frente a otras opciones?

## Ejercicios
- [ ] Crea el endpoint de registro/creación de operario (hashea la contraseña antes de guardar).
- [ ] Crea el endpoint de login: verifica credenciales y emite un JWT firmado con expiración.
- [ ] Añade `firebase/php-jwt` con Composer (una dependencia, no un framework) o implementa la firma HMAC tú mismo si quieres profundizar.
- [ ] Implementa el middleware que valida el token en rutas protegidas.
- [ ] Protege `GET /api/subscribers` y compruébalo con y sin token vía `curl`.

## Criterio de dominio
Un endpoint protegido rechaza peticiones sin token válido (401) y acepta las que llevan un token firmado y no expirado. Puedes explicar la firma de un JWT y por qué no se puede falsificar sin la clave secreta.

## Checklist
- [ ] Contraseñas hasheadas con password_hash
- [ ] Endpoint de login emitiendo JWT
- [ ] Middleware de validación de token
- [ ] Endpoint protegido probado con/sin token
- [ ] Clave secreta del JWT en variable de entorno

## Recursos
- Manual de PHP: [`password_hash`](https://www.php.net/manual/en/function.password-hash.php), [`password_verify`](https://www.php.net/manual/en/function.password-verify.php).
- [jwt.io](https://jwt.io/) — introducción a JWT y depurador de tokens.
- [firebase/php-jwt](https://github.com/firebase/php-jwt) — librería JWT para PHP.
- OWASP Cheat Sheets — [Password Storage](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html), [Authentication](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html).

## Enlaces internos
- Anterior: [cap 05 — API router y capas](05-api-router-capas.md) · Siguiente: [cap 07 — React fundamentos](07-react-fundamentos.md)
- Relacionados: [cap 04b — Hexagonal](04b-arquitectura-hexagonal.md), [cap 08 — Auth en el cliente](08-react-consumo-api.md), [cap 10 — Secretos en producción](10-deploy.md)
- Índice: [00 — Índice](00-INDICE.md)
