# Cap 01 — Fundamentos y arquitectura

## Objetivo
Entender cómo encajan las piezas (React ↔ API PHP ↔ MySQL) y dejar diseñada la arquitectura y el contrato de la API antes de escribir código.

## Por qué
Vienes de Laravel monolítico (Blade + controladores + Eloquent juntos). Aquí separas **frontend** (React, una SPA) de **backend** (API que solo devuelve JSON). Esa frontera clara es el aprendizaje central. Diseñar antes evita retrabajo.

## Subcapítulos
Hazlos en orden. Cada uno tiene sus conceptos, preguntas, ejercicios con entregable, criterio de dominio y checklist.

| # | Subcapítulo | Entregable principal |
|---|-------------|----------------------|
| 01a | [HTTP y el modelo cliente-servidor](01a-http-cliente-servidor.md) | Notas: disección de `curl -v`, tabla de métodos, escenarios → códigos |
| 01b | [Arquitectura del sistema: SPA + API, capas y entornos](01b-arquitectura-sistema.md) | Diagramas dev y prod en `docs/architecture/` |
| 01c | [Orígenes, Same-Origin Policy y CORS](01c-origenes-cors.md) | Notas: predicción de CORS y estrategia dev/prod |
| 01d | [Diseño de la API REST (el contrato)](01d-diseno-api-rest.md) | `docs/api/contract.md` |
| 01e | [Alcance del MVP, monorepo y cierre](01e-mvp-monorepo-cierre.md) | `docs/mvp.md`, estructura del repo, `.gitignore`, diagrama de secuencia |

## Monorepo con FE y BE separados
- Un solo repositorio, dos aplicaciones independientes y desacopladas. No comparten código; solo un contrato: la API JSON (que diseñas en el 01d).
- Estructura raíz:
```
parking/
  backend/      ← app PHP (arquitectura hexagonal, ver cap 04b)
  frontend/     ← app React (Vite, Tailwind, Lucide)
  db/           ← scripts .sql de inicialización
  docker/       ← Dockerfiles y configs (o dentro de cada app)
  docker-compose.yml
  docker-compose.prod.yml
  README.md
```
- Cada app tiene su propio ciclo: `backend` su `composer.json`; `frontend` su `package.json`. Se construyen y despliegan por separado (ver [cap 10](10-deploy.md)).

## Criterio de dominio
Puedes explicar a alguien, sin notas, el recorrido completo de un dato desde que el usuario hace clic en "Guardar abonado" hasta que se escribe en MySQL y vuelve a la pantalla (se demuestra en el 01e, ejercicio 4).

## Checklist
- [ ] [01a — HTTP](01a-http-cliente-servidor.md)
- [ ] [01b — Arquitectura del sistema](01b-arquitectura-sistema.md)
- [ ] [01c — Orígenes y CORS](01c-origenes-cors.md)
- [ ] [01d — Contrato de la API](01d-diseno-api-rest.md)
- [ ] [01e — MVP, monorepo y cierre](01e-mvp-monorepo-cierre.md)

## Enlaces internos
- Siguiente: [cap 02 — Docker](02-docker-entorno-dev.md)
- Relacionados: [cap 04b — Arquitectura hexagonal](04b-arquitectura-hexagonal.md), [cap 05 — API router y capas](05-api-router-capas.md), [cap 10 — Deploy](10-deploy.md)
- Índice: [00 — Índice](00-INDICE.md)
