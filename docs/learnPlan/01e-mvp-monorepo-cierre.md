# Cap 01e — Alcance del MVP, monorepo y cierre del capítulo

> Parte de [cap 01 — Fundamentos y arquitectura](01-fundamentos-y-arquitectura.md) · Anterior: [01d — Diseño de la API REST](01d-diseno-api-rest.md) · Siguiente: [cap 02 — Docker](02-docker-entorno-dev.md)

## Objetivo
Dejar cerrado qué entra en la v1 y qué no, el repositorio preparado para recibir las dos aplicaciones, y demostrar que entiendes el recorrido completo de un dato.

## Por qué
El README ya enumera las 7 features, pero una lista no es un alcance: falta decir **cuándo está terminada** cada una y qué se queda fuera. Sin eso, el MVP crece sin fin. El monorepo, el `.gitignore` y la convención de commits se deciden una vez y se arrastran todo el proyecto.

## Conceptos a dominar
- **MVP**: mínimo que aporta valor de punta a punta. *Must have* vs *nice to have*.
- **Criterios de aceptación**: frases comprobables que dicen cuándo una feature está terminada.
- **Vertical slice**: por qué se construye feature a feature (BD → API → UI) y no capa a capa.
- **Monorepo**: dos apps independientes en un repo; qué comparten (nada de código) y qué no.
- **`.gitignore`**: patrones, alcance por carpeta y qué no debe entrar nunca al repo (dependencias, secretos, artefactos de build, ficheros del IDE).
- **Convención de commits**: por qué un formato fijo (p. ej. *Conventional Commits*) facilita leer el historial.

## Preguntas de autoevaluación
1. ¿Qué diferencia hay entre una feature y su criterio de aceptación?
2. ¿Por qué no conviene construir todo el backend primero y luego todo el frontend?
3. ¿Dónde pondrías el `.gitignore` de `vendor/`: en la raíz o en `backend/`? ¿Qué cambia?
4. ¿Por qué `.env` no se sube al repo pero un `.env.example` sí?
5. ¿Debería estar `.idea/` en el repositorio? Argumenta a favor y en contra.

## Ejercicios
Las respuestas a la autoevaluación y tus dudas van en `docs/notes/01e-mvp-monorepo.md`.

### 1. Alcance del MVP
Entregable: `docs/mvp.md`.
1. Para cada una de las 7 features del README, escribe 2-4 **criterios de aceptación** comprobables (formato sugerido: *"Dado… cuando… entonces…"*).
2. Haz una lista de **nice to have** que conscientemente dejas fuera de la v1 (mínimo 5) y por qué.
3. Ordena las features en el orden en que las construirás y justifica las dependencias (p. ej. ¿puedes hacer suscripciones sin plazas?).

### 2. Estructura del monorepo
1. Crea las carpetas `backend/`, `frontend/`, `db/` y `docker/` según el README. Git no versiona carpetas vacías: investiga cómo se resuelve esto habitualmente.
2. Escribe tu `.gitignore` (raíz, por carpeta o ambos; decide y justifícalo). Debe cubrir como mínimo dependencias de PHP y Node, ficheros `.env`, artefactos de build y ficheros del IDE.
3. Comprueba que funciona: crea un `.env` de prueba y verifica con `git status` y `git check-ignore -v` que se ignora. Bórralo después.

### 3. Convención de commits
Lee la especificación de Conventional Commits, decide si la adoptas (o una variante) y añádela al README en una sección breve. A partir de aquí, todos tus commits la siguen.

### 4. Cierre: el recorrido de un dato
Entregable: `docs/architecture/save-subscriber.mermaid` (diagrama de secuencia).
1. Dibuja un diagrama de **secuencia** del caso "el operario pulsa *Guardar abonado*": React → nginx → PHP-FPM → router → controlador → caso de uso → repositorio → MySQL, y la vuelta hasta que la pantalla se actualiza.
2. Incluye en las flechas el método, la ruta y el código de estado de tu `contract.md`.
3. Añade una rama alternativa (`alt`) para el caso "el documento ya existe".
4. Explícaselo al instructor **sin mirar el diagrama**.

## Criterio de dominio
Es el criterio de dominio del capítulo 01 completo: puedes explicar sin notas el recorrido de un dato desde el clic en "Guardar abonado" hasta MySQL y de vuelta a la pantalla, incluido el caso de error.

## Checklist
- [ ] `docs/mvp.md` con criterios de aceptación y nice to have
- [ ] Estructura `backend/ frontend/ db/ docker/` creada
- [ ] `.gitignore` configurado y verificado
- [ ] Convención de commits decidida y documentada
- [ ] Diagrama de secuencia "Guardar abonado"
- [ ] Recorrido explicado sin notas

## Recursos
- Git — [gitignore](https://git-scm.com/docs/gitignore), [git check-ignore](https://git-scm.com/docs/git-check-ignore).
- [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/).
- Mermaid — [Sequence diagrams](https://mermaid.js.org/syntax/sequenceDiagram.html) (mira `alt`/`else`).
- JetBrains — [Qué ficheros de `.idea` compartir en VCS](https://www.jetbrains.com/help/idea/creating-and-managing-projects.html#share-project-through-vcs).
