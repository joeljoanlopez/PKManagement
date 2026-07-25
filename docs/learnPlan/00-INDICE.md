# Plan de Aprendizaje — Plataforma Parking de Abonados

Proyecto de aprendizaje **sin IA**. React (FE) + PHP nativo (BE) + MySQL, todo en Docker.
Objetivo doble: **aprender el stack** y **construir la plataforma** tú solo.

---

## Cómo usar este libro

- Un capítulo = un módulo de aprendizaje. Sigue el orden.
- Cada capítulo tiene:
  - **Objetivo** — qué sabrás hacer al terminar.
  - **Por qué** — para qué sirve y cómo conecta con lo que ya sabes (Laravel).
  - **Conceptos a dominar** — lista de temas. No avances hasta poder explicarlos con tus palabras.
  - **Preguntas de autoevaluación** — si no sabes responder, vuelve a estudiar.
  - **Ejercicios** — los haces tú, sin copiar soluciones.
  - **Criterio de dominio** — cómo sabes que ya lo tienes.
  - **Checklist** — casillas para trackear en Anytype.
  - **Recursos** — dónde estudiar (fuentes primarias, no tutoriales de terceros).

## Regla de oro

> No copies código que no entiendes. Si algo "funciona por magia", párate y entiende por qué antes de seguir.

## Reglas de trabajo (aplican a todo el proyecto)

- Git desde el primer commit. Commits pequeños y descriptivos.
- `curl` / Postman antes de construir cualquier UI.
- Un concepto nuevo por sesión de estudio.
- Escribe notas propias de cada concepto (en Anytype). Si no puedes explicarlo por escrito, no lo dominas.
- Credenciales y secretos SIEMPRE en variables de entorno, nunca en el código.
- Backend en **arquitectura hexagonal** (cap 04b): dominio aislado de infraestructura.
- **Monorepo** con `../../frontend` y `../../backend` desacoplados; único contrato = la API JSON (cap 01).
- Frontend con **Tailwind** (estilos) y **Lucide** (iconos); estructura feature-based (caps 07-08).
- **Tests y análisis estático** en ambas apps, corriendo en CI (cap 09b). Testea mientras construyes, no al final.

---

## Ruta de capítulos

| # | Capítulo | Bloque |
|---|----------|--------|
| 01 | [Fundamentos y arquitectura](01-fundamentos-y-arquitectura.md) | Diseño |
| 02 | [Docker: entorno de desarrollo](02-docker-entorno-dev.md) | Infra |
| 03 | [PHP nativo: fundamentos](03-php-nativo-fundamentos.md) | Backend |
| 04 | [MySQL + PDO + modelado de datos](04-mysql-pdo-modelado.md) | Backend |
| 04b | [Arquitectura hexagonal (puertos y adaptadores)](04b-arquitectura-hexagonal.md) | Diseño |
| 05 | [API: router y capas](05-api-router-capas.md) | Backend |
| 06 | [Autenticación](06-autenticacion.md) | Backend |
| 07 | [React: fundamentos](07-react-fundamentos.md) | Frontend |
| 08 | [React: consumo de API y estado](08-react-consumo-api.md) | Frontend |
| 09 | [Construcción de features (parking)](09-features-parking.md) | Producto |
| 09b | [Testing y calidad de código](09b-testing-y-calidad.md) | Calidad |
| 10 | [Deploy a servidor](10-deploy.md) | Infra |
| 11 | [Recursos y glosario](11-recursos-y-glosario.md) | Referencia |

## Tablero de progreso global

- [ ] [Cap 01 — Fundamentos y arquitectura](01-fundamentos-y-arquitectura.md)
- [ ] [Cap 02 — Docker dev funcionando](02-docker-entorno-dev.md)
- [ ] [Cap 03 — PHP nativo](03-php-nativo-fundamentos.md)
- [ ] [Cap 04 — MySQL + PDO + modelado](04-mysql-pdo-modelado.md)
- [ ] [Cap 04b — Arquitectura hexagonal](04b-arquitectura-hexagonal.md)
- [ ] [Cap 05 — API router y capas](05-api-router-capas.md)
- [ ] [Cap 06 — Autenticación](06-autenticacion.md)
- [ ] [Cap 07 — React fundamentos](07-react-fundamentos.md)
- [ ] [Cap 08 — React consumo de API](08-react-consumo-api.md)
- [ ] [Cap 09 — Features del parking](09-features-parking.md)
- [ ] [Cap 09b — Testing y calidad de código](09b-testing-y-calidad.md)
- [ ] [Cap 10 — Deploy](10-deploy.md)
- [ ] Proyecto terminado y desplegado 🎉
