# Cap 01b — Arquitectura del sistema: SPA + API, capas y entornos

> Parte de [cap 01 — Fundamentos y arquitectura](01-fundamentos-y-arquitectura.md) · Anterior: [01a — HTTP](01a-http-cliente-servidor.md) · Siguiente: [01c — Orígenes y CORS](01c-origenes-cors.md)

## Objetivo
Tener dibujada la arquitectura de TU sistema en desarrollo y en producción (piezas, puertos y quién habla con quién) y saber qué responsabilidad tiene cada capa del backend.

## Por qué
En Laravel + Blade, el navegador pide una URL y recibe HTML ya montado: todo vive en un proceso. Aquí hay dos aplicaciones distintas que solo se hablan por HTTP + JSON, y varios contenedores. Si no tienes el mapa claro, en el [cap 02](02-docker-entorno-dev.md) no sabrás qué estás configurando.

## Conceptos a dominar
- **Render en servidor (Blade)** vs **SPA**: quién genera el HTML, qué descarga el navegador la primera vez y qué viaja en cada interacción posterior.
- **Por qué el navegador nunca habla con MySQL**: qué pieza es la frontera y qué protege.
- **Piezas del backend en ejecución**: nginx (servidor web / proxy), PHP-FPM (ejecuta PHP), MySQL. Por qué nginx no ejecuta PHP.
- **Capas del backend** (nivel reconocimiento; el detalle hexagonal llega en el [cap 04b](04b-arquitectura-hexagonal.md)): router → controlador → caso de uso → repositorio → BD. Su equivalente en Laravel.
- **Entornos**: desarrollo (Vite dev server, bind mounts, puertos expuestos en tu máquina) vs producción (build estático del frontend servido por nginx, `/api` por proxy, HTTPS). Qué cambia y qué debe ser igual (*dev/prod parity*).
- **Configuración por entorno** en variables de entorno, no en el código.

## Preguntas de autoevaluación
1. En una SPA, ¿qué recibe el navegador en la primera carga? ¿Y cuando el usuario guarda un formulario?
2. ¿Por qué React no puede ver tu base de datos directamente? Da al menos dos razones (técnica y de seguridad).
3. ¿Qué hace nginx y qué hace PHP-FPM? ¿Por qué no basta con uno?
4. Pon el equivalente en Laravel de cada capa: router, controlador, caso de uso, repositorio.
5. ¿Qué tres cosas cambian entre tu entorno de desarrollo y el de producción?
6. ¿Por qué la contraseña de MySQL no puede estar en el código aunque el repo sea privado?

## Ejercicios
Las respuestas escritas van en `docs/notes/01b-architecture.md`; los diagramas, en `docs/architecture/` en Mermaid. Ya tienes `dockerArquitecture.mermaid`: úsalo como punto de partida y amplíalo.

### 1. Monolito vs SPA
Escribe en `docs/notes/01b-architecture.md` dos secuencias numeradas (máximo 8 pasos cada una) de lo que pasa cuando un usuario pulsa "Guardar":
- en una app Laravel + Blade con formulario clásico;
- en tu SPA React + API.

Marca en cada paso qué viaja por la red (HTML, JSON, redirección…).

### 2. Diagrama de desarrollo
Dibuja los componentes del entorno de **desarrollo**:
1. Lista primero las piezas: navegador, Vite dev server, nginx, PHP-FPM, MySQL y volumen de datos.
2. Para cada una, apunta en qué puerto escucha **dentro** de su contenedor y cuál expones en tu máquina (si lo expones). Investiga los puertos por defecto en la doc oficial de cada imagen o herramienta.
3. Dibuja las flechas de comunicación y etiquétalas con el protocolo (HTTP, FastCGI, protocolo MySQL).
4. Responde: ¿qué piezas son accesibles desde tu navegador y cuáles solo desde la red interna de Docker?

### 3. Diagrama de producción
Repite el ejercicio 2 para **producción** según el README: no hay Vite, nginx sirve el build y hace de proxy de `/api`, hay HTTPS. Señala qué ha desaparecido, qué es nuevo y qué puertos quedan abiertos a Internet.

### 4. Capas
Toma una acción concreta ("listar abonados") y escribe qué hace **cada capa** del backend y qué **no** debe hacer. Ejemplo de formato: *"El controlador lee X y devuelve Y; no debe Z."*

## Criterio de dominio
Puedes explicar, sin notas y señalando tu diagrama, por qué puertos y protocolos pasa una petición desde el navegador hasta MySQL en desarrollo y en producción.

## Checklist
- [ ] Comparación monolito vs SPA escrita
- [ ] Diagrama de desarrollo con puertos y protocolos
- [ ] Diagrama de producción
- [ ] Responsabilidad de cada capa definida

## Recursos
- MDN — [SPA (glosario)](https://developer.mozilla.org/en-US/docs/Glossary/SPA).
- nginx — [Beginner's guide](https://nginx.org/en/docs/beginners_guide.html) (sección *Setting Up FastCGI Proxying*).
- PHP — [FastCGI Process Manager (FPM)](https://www.php.net/manual/en/install.fpm.php).
- Docker Hub — imágenes oficiales [`nginx`](https://hub.docker.com/_/nginx), [`php`](https://hub.docker.com/_/php), [`mysql`](https://hub.docker.com/_/mysql) (puertos por defecto).
- Vite — [Server options](https://vite.dev/config/server-options) (puerto por defecto).
- The Twelve-Factor App — [III. Config](https://12factor.net/config) y [X. Dev/prod parity](https://12factor.net/dev-prod-parity).
- Mermaid — [Flowchart](https://mermaid.js.org/syntax/flowchart.html) (subgrafos para agrupar contenedores).
