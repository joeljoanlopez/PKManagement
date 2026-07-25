# Cap 03 — PHP nativo: fundamentos

## Objetivo
Escribir PHP moderno sin framework: manejo de peticiones, respuestas JSON, autoloading con Composer, y buen manejo de errores.

## Por qué
Laravel te ocultaba casi todo esto. Al hacerlo a mano entenderás qué hace un framework por debajo. No es "PHP antiguo": es PHP moderno (8.x) sin la capa de Laravel.

## Conceptos a dominar
- **PHP moderno 8.x**: tipos en parámetros y retornos, `declare(strict_types=1)`, enums, `readonly`, arrow functions, null safe operator.
- **Superglobales**: `$_SERVER` (método, URI, headers), `$_GET`, `$_POST`, `$_FILES`.
- **Leer el body crudo de una petición**: `php://input` + `json_decode` (clave para una API que recibe JSON).
- **Enviar respuestas**: `header()`, códigos de estado con `http_response_code()`, `json_encode`.
- **Composer**: `composer init`, `require`, autoloading **PSR-4** (mapear namespaces a carpetas). Aunque sea "nativo", Composer es estándar, no un framework.
- **Namespaces** y organización en clases.
- **Manejo de errores**: excepciones, `try/catch/finally`, excepciones propias, `set_exception_handler` y `set_error_handler` para capturar todo y responder JSON limpio.
- **Variables de entorno**: leer configuración con `getenv()` / `$_ENV` (nunca hardcodear credenciales).
- **PSR-12**: estilo de código estándar de la comunidad PHP.

## Preguntas de autoevaluación
1. ¿Por qué `$_POST` NO sirve para leer un body JSON? ¿Qué usas en su lugar?
2. ¿Qué hace `declare(strict_types=1)` y por qué lo querrías?
3. ¿Cómo mapea PSR-4 un namespace `App\Controllers\SubscriberController` a un archivo?
4. ¿Cómo capturas cualquier excepción no manejada y devuelves un JSON con código 500 en vez de un error feo de PHP?
5. ¿Cómo lees la contraseña de la BD desde una variable de entorno inyectada por Docker?

## Ejercicios
- [ ] Inicializa Composer en `backend/` y configura autoloading PSR-4 para tu namespace `App\`.
- [ ] Crea una función/clase que lea el body JSON de la petición y lo devuelva como array asociativo (maneja el caso de JSON inválido).
- [ ] Crea un helper para responder JSON con un código de estado dado.
- [ ] Registra un manejador global de excepciones que convierta cualquier error en una respuesta JSON coherente.
- [ ] Lee 3 variables de entorno (host, usuario, password de BD) y verifica que llegan desde Docker.

## Criterio de dominio
Puedes recibir una petición con body JSON, procesarla y responder JSON con el código HTTP correcto, y ningún error tira una traza de PHP en crudo al cliente.

## Checklist
- [ ] Composer + autoloading PSR-4 funcionando
- [ ] Lectura de body JSON
- [ ] Helper de respuesta JSON
- [ ] Manejador global de errores/excepciones
- [ ] Configuración por variables de entorno

## Recursos
- [PHP The Right Way](https://phptherightway.com/) — panorama de buenas prácticas.
- Manual de PHP: [superglobales](https://www.php.net/manual/en/language.variables.superglobals.php), [`json_encode`](https://www.php.net/manual/en/function.json-encode.php) / [`json_decode`](https://www.php.net/manual/en/function.json-decode.php), [excepciones](https://www.php.net/manual/en/language.exceptions.php).
- Composer — [autoloading](https://getcomposer.org/doc/04-schema.md#autoload) y [documentación general](https://getcomposer.org/doc/).
- PHP-FIG — [PSR-4 (autoloading)](https://www.php-fig.org/psr/psr-4/) y [PSR-12 (estilo)](https://www.php-fig.org/psr/psr-12/).

## Enlaces internos
- Anterior: [cap 02 — Docker](02-docker-entorno-dev.md) · Siguiente: [cap 04 — MySQL + PDO](04-mysql-pdo-modelado.md)
- Relacionados: [cap 04b — Arquitectura hexagonal](04b-arquitectura-hexagonal.md), [cap 05 — API router y capas](05-api-router-capas.md), [cap 09b — Testing y calidad](09b-testing-y-calidad.md)
- Índice: [00 — Índice](00-INDICE.md)
