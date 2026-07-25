# Cap 09b — Testing y calidad de código

## Objetivo
Asegurar la calidad con tests (unitarios, integración y, donde aporte, end-to-end) y análisis estático, tanto en backend como en frontend. Automatizar todo para que se ejecute solo.

## Por qué
Sin framework que te dé andamiaje, la calidad depende de ti. Los tests te dan confianza para refactorizar; el análisis estático caza errores y malos olores antes de ejecutar nada. Tu arquitectura hexagonal ([cap 04b](04b-arquitectura-hexagonal.md)) hace el dominio trivial de testear: sin BD, sin HTTP, puro. Aprovéchalo.

> Idealmente introduce los tests **desde el [cap 05](05-api-router-capas.md) / [cap 09](09-features-parking.md)**, no al final. Este capítulo agrupa la teoría; aplícala mientras construyes cada slice (TDD ligero o test-after, pero testea).

---

## Conceptos transversales a dominar
- **Pirámide de tests**: muchos unitarios (rápidos, aislados), menos de integración, pocos end-to-end (lentos, frágiles). Entiende el porqué de las proporciones.
- **Tipos de test**:
  - *Unitario*: una unidad aislada (una función, un caso de uso, un componente) con sus dependencias sustituidas.
  - *Integración*: varias piezas juntas contra recursos reales (ej: repositorio contra MySQL real; componente + cliente API mockeado).
  - *End-to-end (E2E)*: el sistema completo desde el navegador. Opcional pero valioso para flujos críticos.
- **Test doubles**: dummy, stub, fake, mock, spy. Sabe cuál usar. (Tu repositorio en memoria del [cap 04b](04b-arquitectura-hexagonal.md) es un *fake* perfecto).
- **Patrón AAA**: Arrange, Act, Assert. Un test = una razón para fallar.
- **Qué testear**: comportamiento observable y reglas de negocio, NO detalles de implementación (para poder refactorizar sin romper tests).
- **Cobertura (coverage)**: útil como señal, engañosa como objetivo. 100% no significa "bien testeado". No persigas el número.
- **Tests deterministas**: sin dependencia de fecha/hora real, orden ni estado compartido. Aíslalos.
- **Análisis estático**: detectar errores sin ejecutar código (tipos, nulos, código muerto, estilo). Complementa a los tests.
- **Integración continua (CI)**: ejecutar tests + análisis en cada push automáticamente.

---

## Backend (PHP)

### Testing con PHPUnit
- Instalación de PHPUnit vía Composer (dependencia de desarrollo).
- Estructura: carpeta `tests/` espejando `src/`; configuración en `phpunit.xml`.
- **Tests unitarios del dominio y casos de uso**: aquí brilla la hexagonal. Testea entidades y reglas de negocio sin nada de infraestructura. Inyecta el repositorio *en memoria* (fake) en los casos de uso.
- **Data providers**, `setUp`/`tearDown`, aserciones, testear excepciones esperadas.
- **Tests de integración**: repositorio PDO contra una **base de datos de test real** (un MySQL de test en Docker, o esquema separado). Cada test parte de un estado conocido (transacción con rollback, o recrear/limpiar tablas).
- **Tests de API (funcionales)**: golpear los endpoints y verificar código HTTP + JSON de respuesta. Comprueban router + controlador + caso de uso + repositorio juntos. Decide si contra la app levantada (HTTP real) o invocando el kernel de la app en proceso.

### Análisis estático (PHP)
- **PHPStan** (o **Psalm**): detecta errores de tipos, nulos, métodos inexistentes, código inalcanzable. Empieza en un nivel bajo y sube gradualmente hasta el máximo. Objetivo: nivel alto sin errores.
- **PHP-CS-Fixer** o **PHP_CodeSniffer**: fuerza el estilo **PSR-12** automáticamente.
- **Composer scripts**: define atajos (`composer test`, `composer stan`, `composer cs`) para correr todo con un comando.

---

## Frontend (React)

### Testing con Vitest + React Testing Library
- **Vitest** como runner (se integra nativo con Vite). Alternativa clásica: Jest.
- **React Testing Library (RTL)**: filosofía = testear como lo usaría el usuario (buscar por rol/texto, disparar eventos), NO inspeccionar estado interno ni implementación.
- **Tests unitarios**: componentes de UI reutilizables (botón, input, tabla), hooks, y lógica pura (formateos, cálculos).
- **Tests de integración**: una feature completa (ej: pantalla de abonados) renderizada, con el **cliente API mockeado**. Usa **MSW (Mock Service Worker)** para interceptar peticiones HTTP y simular respuestas del backend de forma realista.
- Testea estados de carga, error y éxito; interacción de formularios; navegación.
- `user-event` para simular interacción real del usuario.

### E2E (opcional, para flujos críticos)
- **Playwright** (recomendado) o Cypress: navegador real contra la app levantada (frontend + backend + BD de test).
- Reserva E2E para caminos críticos: login → crear abonado → asignar plaza → registrar pago. No para todo.

### Análisis estático (Frontend)
- **TypeScript** (muy recomendado): el mejor análisis estático del FE. Aunque tu API sea PHP, tipar el frontend caza una clase enorme de bugs y documenta los datos de la API. Considera adoptarlo desde el cap 07.
- **ESLint**: reglas de calidad y errores comunes de JS/React (`eslint-plugin-react-hooks` para las reglas de hooks; `typescript-eslint` si usas TS).
- **Prettier**: formateo automático consistente.
- **npm scripts**: `npm run test`, `npm run lint`, `npm run typecheck`, `npm run format`.

---

## Integración continua (CI)
- Configura **GitHub Actions** (u otro) para que en cada push/PR se ejecuten: tests BE + análisis BE + tests FE + lint/typecheck FE.
- Un job por app (backend, frontend) aprovechando el monorepo.
- Servicio de MySQL en el runner para los tests de integración del backend.
- Falla el build si algo no pasa. Así la calidad no depende de tu memoria.

---

## Preguntas de autoevaluación
1. ¿Por qué la pirámide tiene muchos unitarios y pocos E2E?
2. ¿Por qué tu dominio hexagonal es tan fácil de testear? ¿Qué double usas para el repositorio?
3. ¿Qué diferencia hay entre testear "comportamiento" y "implementación", y por qué importa al refactorizar?
4. ¿Por qué 100% de cobertura no garantiza calidad?
5. ¿Cómo aíslas los tests de integración del backend para que sean deterministas?
6. ¿Qué aporta MSW frente a mockear `fetch` a mano en los tests del FE?
7. ¿Qué caza el análisis estático que los tests no, y viceversa?
8. ¿Por qué RTL desaconseja buscar elementos por clase CSS o id de implementación?

## Ejercicios
### Backend
- [ ] Instala y configura PHPUnit.
- [ ] Escribe tests unitarios de una entidad de dominio y de un caso de uso (con repositorio en memoria).
- [ ] Escribe un test de integración de un repositorio PDO contra una BD de test, con estado limpio por test.
- [ ] Escribe un test funcional de un endpoint (código HTTP + JSON).
- [ ] Añade PHPStan/Psalm; sube el nivel hasta que quede sin errores.
- [ ] Añade PHP-CS-Fixer/PHPCS con PSR-12 y arregla el estilo.
- [ ] Crea composer scripts para test, análisis y estilo.

### Frontend
- [ ] Configura Vitest + React Testing Library.
- [ ] Testea un componente de UI reutilizable por comportamiento.
- [ ] Testea una feature completa con el cliente API mockeado vía MSW (carga/error/éxito).
- [ ] Configura ESLint + Prettier (+ TypeScript y typecheck si lo adoptas).
- [ ] Crea npm scripts para test, lint, typecheck y format.
- [ ] (Opcional) Un test E2E con Playwright del flujo crítico login → alta de abonado.

### CI
- [ ] Configura un pipeline que ejecute tests + análisis de ambas apps en cada push, con MySQL de test para el backend.

## Criterio de dominio
Puedes refactorizar dominio o UI y saber en segundos si rompiste algo. El análisis estático pasa en nivel alto sin errores en ambas apps, y el pipeline de CI bloquea cualquier regresión antes de merge.

## Checklist
- [ ] PHPUnit: unitarios de dominio/casos de uso
- [ ] PHPUnit: integración contra BD de test
- [ ] PHPUnit: tests funcionales de endpoints
- [ ] PHPStan/Psalm en nivel alto, limpio
- [ ] PHP-CS-Fixer/PHPCS con PSR-12
- [ ] Composer scripts (test/stan/cs)
- [ ] Vitest + RTL configurados
- [ ] Tests de componentes por comportamiento
- [ ] Tests de integración FE con MSW
- [ ] ESLint + Prettier (+ TypeScript)
- [ ] npm scripts (test/lint/typecheck/format)
- [ ] (Opcional) E2E de flujo crítico con Playwright
- [ ] CI ejecutando todo en cada push

## Recursos
- [PHPUnit](https://docs.phpunit.de/) — documentación oficial.
- [PHPStan](https://phpstan.org/) y/o [Psalm](https://psalm.dev/) — análisis estático (guías de niveles).
- [PHP-CS-Fixer](https://cs.symfony.com/) / [PHP_CodeSniffer](https://github.com/PHPCSStandards/PHP_CodeSniffer) — estilo de código.
- [Vitest](https://vitest.dev/) — runner de tests.
- Testing Library — [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/), [`user-event`](https://testing-library.com/docs/user-event/intro/), [queries](https://testing-library.com/docs/queries/about/).
- [MSW (Mock Service Worker)](https://mswjs.io/) — mock de API.
- [Playwright](https://playwright.dev/) — E2E.
- [ESLint](https://eslint.org/), [Prettier](https://prettier.io/), [TypeScript](https://www.typescriptlang.org/), [typescript-eslint](https://typescript-eslint.io/).
- Martin Fowler — [The Practical Test Pyramid](https://martinfowler.com/articles/practical-test-pyramid.html), [Test Double](https://martinfowler.com/bliki/TestDouble.html).
- [GitHub Actions](https://docs.github.com/en/actions) — CI.

## Enlaces internos
- Anterior: [cap 09 — Features del parking](09-features-parking.md) · Siguiente: [cap 10 — Deploy](10-deploy.md)
- Testea lo de: [cap 04b — Dominio hexagonal](04b-arquitectura-hexagonal.md), [cap 04 — Repositorios PDO](04-mysql-pdo-modelado.md), [cap 05 — Endpoints](05-api-router-capas.md), [cap 07](07-react-fundamentos.md) y [cap 08 — React](08-react-consumo-api.md)
- Índice: [00 — Índice](00-INDICE.md)
