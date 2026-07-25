# PKManagement — Plataforma de gestión de abonados de parking

Aplicación de gestión de clientes de un parking (abonados, vehículos, plazas, suscripciones y pagos),
construida **100% sin IA**: cada línea de código se escribe y se entiende a mano.

## Motivo

Doble objetivo: **aprender el stack de verdad** y **construir la plataforma**.

Viniendo de Laravel monolítico (Blade + controladores + Eloquent), este proyecto quita el framework
del medio para entender qué hace por debajo: routing a mano, PDO en vez de Eloquent, JWT en vez de
Sanctum, React en vez de Blade. Sin generación automática de código y sin copiar lo que no se entiende.

> **Regla de oro:** no copies código que no entiendes. Si algo "funciona por magia", párate y entiende
> por qué antes de seguir.

Reglas de trabajo del proyecto:

- Git desde el primer commit; commits pequeños y descriptivos.
- `curl` / Postman antes de construir cualquier UI.
- Secretos siempre en variables de entorno, nunca en el código.
- Backend en arquitectura hexagonal: dominio aislado de la infraestructura.
- Monorepo con `frontend/` y `backend/` desacoplados; único contrato = la API JSON.
- Tests y análisis estático en ambas apps, corriendo en CI. Testear mientras se construye, no al final.

## Stack

| Capa | Tecnología |
|------|-----------|
| Frontend | React + Vite, Tailwind CSS, Lucide, React Router, TanStack Query |
| Backend | PHP 8.3 nativo (sin framework), Composer con autoloading PSR-4 |
| Base de datos | MySQL con acceso vía PDO y prepared statements |
| Infra | Docker Compose (nginx + PHP-FPM + MySQL), deploy en VPS con HTTPS |
| Calidad | PHPUnit + PHPStan + PHP-CS-Fixer · Vitest + RTL + MSW + ESLint · CI en GitHub Actions |

## Arquitectura

SPA React ↔ API REST JSON ↔ MySQL. Dos aplicaciones independientes en un monorepo, acopladas solo
por el contrato de la API.

```
parking/
  backend/      ← app PHP (arquitectura hexagonal)
    src/
      Domain/           ← entidades, value objects, excepciones de negocio, interfaces de puertos
      Application/      ← casos de uso, DTOs
      Infrastructure/   ← adaptadores: HTTP (router, controladores), persistencia PDO
  frontend/     ← app React (Vite, Tailwind, Lucide), estructura feature-based
  db/           ← scripts .sql de inicialización
  docker/       ← Dockerfiles y configuración
  docker-compose.yml
  docker-compose.prod.yml
```

Regla de dependencia: las capas externas dependen de las internas, nunca al revés. El dominio no
conoce HTTP ni SQL; los repositorios son interfaces en el dominio con implementación PDO en
infraestructura; el controlador es un adaptador de entrada que traduce HTTP → caso de uso → JSON.

## Alcance funcional (MVP)

Se construye por **vertical slices**: cada feature de punta a punta (BD → API → UI) antes de pasar a
la siguiente.

1. **Operarios y autenticación** — login con `password_hash` + JWT, middleware de rutas protegidas.
2. **Abonados** — CRUD con búsqueda y paginación, estados (activo/suspendido/cancelado), documento único.
3. **Vehículos** — N vehículos por abonado, gestionados desde su ficha.
4. **Plazas** — CRUD (código, planta, estado) y vista de ocupación.
5. **Suscripciones** — abono mensual con plaza, precio y fechas; no se asigna una plaza ya ocupada.
6. **Pagos** — registro por periodo, listados por abonado y periodo, detección de impagos.
7. **Dashboard** — plazas ocupadas/libres, abonados activos, ingresos del mes, impagos pendientes.

Criterio de terminado: un operario puede dar de alta un abonado con su vehículo, asignarle plaza y
abono mensual, registrar sus pagos y ver el estado del parking, con auth, validación en backend y
datos persistidos.

## Contenido del repositorio

Ahora mismo el repo contiene el **plan de aprendizaje y diseño** en [`docs/`](docs/learnPlan/00-INDICE.md). El
código de `backend/` y `frontend/` se va añadiendo capítulo a capítulo.

Cada capítulo incluye objetivo, por qué, conceptos a dominar, preguntas de autoevaluación, ejercicios,
criterio de dominio, checklist y recursos (fuentes primarias, no tutoriales de terceros).

| # | Capítulo | Bloque |
|---|----------|--------|
| 01 | [Fundamentos y arquitectura](docs/learnPlan/01-fundamentos-y-arquitectura.md) | Diseño |
| 02 | [Docker: entorno de desarrollo](docs/learnPlan/02-docker-entorno-dev.md) | Infra |
| 03 | [PHP nativo: fundamentos](docs/learnPlan/03-php-nativo-fundamentos.md) | Backend |
| 04 | [MySQL + PDO + modelado de datos](docs/learnPlan/04-mysql-pdo-modelado.md) | Backend |
| 04b | [Arquitectura hexagonal (puertos y adaptadores)](docs/learnPlan/04b-arquitectura-hexagonal.md) | Diseño |
| 05 | [API: router y capas](docs/learnPlan/05-api-router-capas.md) | Backend |
| 06 | [Autenticación](docs/learnPlan/06-autenticacion.md) | Backend |
| 07 | [React: fundamentos](docs/learnPlan/07-react-fundamentos.md) | Frontend |
| 08 | [React: consumo de API y estado](docs/learnPlan/08-react-consumo-api.md) | Frontend |
| 09 | [Construcción de features (parking)](docs/learnPlan/09-features-parking.md) | Producto |
| 09b | [Testing y calidad de código](docs/learnPlan/09b-testing-y-calidad.md) | Calidad |
| 10 | [Deploy a servidor](docs/learnPlan/10-deploy.md) | Infra |
| 11 | [Recursos y glosario](docs/learnPlan/11-recursos-y-glosario.md) | Referencia |

## Puesta en marcha

Pendiente hasta que exista el entorno del [cap 02](docs/learnPlan/02-docker-entorno-dev.md). El objetivo es
levantar todo con un solo comando:

```bash
docker compose up
```

- nginx (front controller hacia PHP-FPM por FastCGI en el 9000)
- PHP-FPM 8.3 con `pdo_mysql`
- MySQL con volumen persistente e inicialización desde `db/*.sql`

Frontend en dev con Vite (`npm run dev`); en producción, build estático servido por nginx con proxy
de `/api` hacia PHP-FPM.

## Deploy

VPS con Docker: multi-stage build del frontend, código dentro de la imagen (sin bind mounts), HTTPS
con Let's Encrypt, secretos en `.env` fuera del repo, volumen y dumps periódicos de MySQL,
`restart: unless-stopped`. Detalle en el [cap 10](docs/learnPlan/10-deploy.md).
