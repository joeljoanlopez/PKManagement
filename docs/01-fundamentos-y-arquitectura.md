# Cap 01 — Fundamentos y arquitectura

## Objetivo
Entender cómo encajan las piezas (React ↔ API PHP ↔ MySQL) y dejar diseñada la arquitectura antes de escribir código.

## Por qué
Vienes de Laravel monolítico (Blade + controladores + Eloquent juntos). Aquí separas **frontend** (React, una SPA) de **backend** (API que solo devuelve JSON). Esa frontera clara es el aprendizaje central. Diseñar antes evita retrabajo.

## Conceptos a dominar
- Diferencia entre **app monolítica con render en servidor** (Laravel + Blade) y **SPA + API REST**.
- Qué es una **API REST**: recursos, métodos HTTP (GET/POST/PUT/PATCH/DELETE), códigos de estado (2xx, 4xx, 5xx), idempotencia.
- Qué viaja entre FE y BE: **JSON**, no HTML.
- Concepto de **origen** (origin = esquema + host + puerto) y por qué aparecerá CORS.
- Arquitectura en **capas** del backend: Router → Controller → Servicio/Repositorio → BD. (Reconócela: es lo que Laravel hace por ti). Se profundiza con **hexagonal** en el [cap 04b](04b-arquitectura-hexagonal.md).
- Separación de entornos: **desarrollo** (tu máquina, Docker) vs **producción** (servidor).

### Monorepo con FE y BE separados
- Un solo repositorio, dos aplicaciones independientes y desacopladas. No comparten código; solo un contrato: la API JSON.
- Estructura raíz sugerida:
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
- Ventaja: cambiar el frontend no toca el backend y viceversa; el único acoplamiento es el contrato de la API.
- Decide desde el inicio: convención de commits, y si quieres, un `.gitignore` por carpeta (`vendor/`, `node_modules/`, `.env`).

## Preguntas de autoevaluación
1. ¿Por qué React no puede "ver" tu base de datos directamente?
2. ¿Qué código HTTP devolverías al crear un abonado con éxito? ¿Y si el email ya existe? ¿Y si falta un campo obligatorio?
3. ¿Qué significa que un endpoint sea idempotente? ¿Cuáles de tus operaciones lo serán?
4. ¿Por qué `localhost:5173` (React) y `localhost:8080` (API) se consideran orígenes distintos?
5. ¿Qué responsabilidad tiene cada capa del backend?

## Ejercicios
- [ ] Dibuja el diagrama de arquitectura de TU proyecto (contenedores, puertos, flechas de comunicación).
- [ ] Lista los **recursos** de tu dominio (abonados, vehículos, plazas, suscripciones, pagos, usuarios). Para cada uno, define qué endpoints REST necesitarás.
- [ ] Escribe una tabla: acción de usuario → método HTTP + ruta → código de respuesta esperado.
- [ ] Define el alcance del MVP: ¿qué features son imprescindibles para la v1? ¿Cuáles son "nice to have"?

## Criterio de dominio
Puedes explicar a alguien, sin notas, el recorrido completo de un dato desde que el usuario hace clic en "Guardar abonado" hasta que se escribe en MySQL y vuelve a la pantalla.

## Checklist
- [ ] Diagrama de arquitectura hecho
- [ ] Lista de recursos y endpoints definida
- [ ] Alcance del MVP escrito
- [ ] Repo git (monorepo) creado con estructura frontend/ + backend/ + db/ + docker
- [ ] .gitignore configurado (vendor/, node_modules/, .env)

## Recursos
- MDN — [Overview of HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview), [HTTP response status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status), [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).
- [REST API design](https://restfulapi.net/) — principios de diseño de APIs REST.
- Repasa cómo Laravel resuelve routing y controllers — vas a replicarlo a mano (se detalla en [cap 05](05-api-router-capas.md)).

## Enlaces internos
- Siguiente: [cap 02 — Docker](02-docker-entorno-dev.md)
- Relacionados: [cap 04b — Arquitectura hexagonal](04b-arquitectura-hexagonal.md), [cap 05 — API router y capas](05-api-router-capas.md), [cap 10 — Deploy](10-deploy.md)
- Índice: [00 — Índice](00-INDICE.md)
