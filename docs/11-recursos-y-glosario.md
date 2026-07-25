# Cap 11 — Recursos y glosario

> Índice: [00 — Índice](00-INDICE.md) · Anterior: [cap 10 — Deploy](10-deploy.md)

## Fuentes primarias (prioriza estas sobre tutoriales sueltos)

### Docker
- [Docker](https://docs.docker.com/) y [Docker Compose](https://docs.docker.com/compose/).
- Docker Hub: [`php`](https://hub.docker.com/_/php), [`mysql`](https://hub.docker.com/_/mysql), [`nginx`](https://hub.docker.com/_/nginx).
- Docker — [multi-stage builds](https://docs.docker.com/build/building/multi-stage/).

### PHP nativo
- [Manual oficial de PHP](https://www.php.net/manual/en/).
- [PHP The Right Way](https://phptherightway.com/).
- [Composer](https://getcomposer.org/doc/).
- PHP-FIG — [PSR-4](https://www.php-fig.org/psr/psr-4/), [PSR-12](https://www.php-fig.org/psr/psr-12/).

### MySQL
- [Documentación oficial de MySQL](https://dev.mysql.com/doc/) (tipos, FKs, índices, JOINs, agregación).

### React / Frontend
- [react.dev](https://react.dev/) — documentación oficial (la mejor fuente para aprender).
- [Vite](https://vite.dev/).
- [TanStack Query](https://tanstack.com/query/latest).
- [React Router](https://reactrouter.com/).
- [Tailwind CSS](https://tailwindcss.com/docs) — [instalación con Vite](https://tailwindcss.com/docs/installation/using-vite).
- [lucide.dev](https://lucide.dev/) — iconos y `lucide-react`.

### Arquitectura
- Alistair Cockburn — [Hexagonal Architecture (Ports and Adapters)](https://alistair.cockburn.us/hexagonal-architecture/).
- Robert C. Martin — [The Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html).
- Ejemplos de hexagonal en PHP (repos de referencia).

### Testing y calidad
- [PHPUnit](https://docs.phpunit.de/), [PHPStan](https://phpstan.org/), [Psalm](https://psalm.dev/), [PHP-CS-Fixer](https://cs.symfony.com/).
- [Vitest](https://vitest.dev/), [Testing Library](https://testing-library.com/docs/react-testing-library/intro/), [MSW](https://mswjs.io/), [Playwright](https://playwright.dev/).
- [ESLint](https://eslint.org/), [Prettier](https://prettier.io/), [TypeScript](https://www.typescriptlang.org/), [typescript-eslint](https://typescript-eslint.io/).
- Martin Fowler — [Practical Test Pyramid](https://martinfowler.com/articles/practical-test-pyramid.html), [Test Double](https://martinfowler.com/bliki/TestDouble.html).

### Deploy
- [Let's Encrypt](https://letsencrypt.org/) / [Certbot](https://certbot.eff.org/) / [Caddy](https://caddyserver.com/).
- [Initial Server Setup (hardening básico)](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-22-04).
- [GitHub Actions (CI)](https://docs.github.com/en/actions).

### Web / HTTP / Seguridad
- MDN — [HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP), [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS), [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API).
- [jwt.io](https://jwt.io/).
- [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/).

---

## Glosario

- **SPA (Single Page Application)**: app web que carga una vez y actualiza la vista sin recargar; React genera el HTML en el navegador.
- **API REST**: interfaz que expone recursos vía HTTP y devuelve datos (JSON), sin generar la UI.
- **Front controller**: único punto de entrada que recibe todas las peticiones y las enruta.
- **Router**: componente que asocia método + ruta a un manejador.
- **Middleware**: función que se ejecuta antes (o después) del manejador principal (auth, CORS, logging).
- **CORS**: mecanismo del navegador que controla peticiones entre orígenes distintos.
- **Preflight**: petición `OPTIONS` que el navegador envía antes de ciertas peticiones cross-origin.
- **PDO**: capa de acceso a bases de datos de PHP; usa prepared statements.
- **Prepared statement**: consulta parametrizada que separa SQL de datos y evita inyección.
- **Inyección SQL**: ataque que inserta SQL malicioso vía entradas no saneadas.
- **Transacción**: conjunto de operaciones que se confirman todas o ninguna (commit/rollback).
- **JWT**: token firmado (header.payload.signature) para autenticación sin estado.
- **Hash de contraseña**: transformación unidireccional para almacenar contraseñas de forma segura.
- **PSR-4**: estándar de autoloading que mapea namespaces a rutas de archivos.
- **Vertical slice**: feature construida de punta a punta (BD → API → UI) antes de pasar a la siguiente.
- **Arquitectura hexagonal (puertos y adaptadores)**: patrón que aísla el dominio de la infraestructura; las dependencias apuntan hacia el dominio.
- **Puerto**: interfaz que define un punto de interacción del dominio (entrada o salida).
- **Adaptador**: implementación concreta que conecta un puerto con el mundo exterior (HTTP, PDO, etc.).
- **Caso de uso**: acción de negocio orquestada en la capa de aplicación, sin conocer HTTP ni SQL.
- **Composition root**: punto único donde se cablean (inyectan) las dependencias concretas.
- **Monorepo**: un repositorio que alberga varias apps (aquí, frontend y backend) desacopladas.
- **Utility-first CSS**: enfoque de CSS por clases de utilidad componibles (Tailwind).
- **Multi-stage build**: Dockerfile con varias etapas; una construye, otra solo lleva el resultado (imagen final más ligera).
- **Vite**: herramienta de build y dev-server para el frontend.
- **PHP-FPM**: gestor de procesos que ejecuta PHP; nginx le pasa las peticiones vía FastCGI.

---

## Plantilla de nota de aprendizaje (cópiala por cada concepto)

```
## Concepto: <nombre>
- Qué es (con mis palabras):
- Para qué sirve:
- Cómo se relaciona con lo que ya sabía (Laravel):
- Ejemplo mínimo que hice:
- Dudas pendientes:
```

## Plantilla de bitácora diaria

```
## Fecha:
- Capítulo/slice en el que trabajé:
- Qué construí/aprendí:
- Qué se me atascó y cómo lo resolví:
- Siguiente paso:
```
