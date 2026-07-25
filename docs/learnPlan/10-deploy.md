# Cap 10 — Deploy a servidor

## Objetivo
Publicar la plataforma en un servidor (VPS) con Docker, sirviendo el build de React y la API PHP, con HTTPS.

## Por qué
Desarrollar en local no es suficiente: el objetivo es servir la plataforma desde un servidor. Aquí aprendes las diferencias entre dev y prod, y cómo desplegar de forma reproducible.

## Conceptos a dominar
- **Diferencias dev vs prod**:
  - React en dev usa Vite dev-server; en prod se hace **build** (`vite build`) → archivos estáticos que sirve nginx.
  - Sin bind mounts de código en prod: el código va **dentro** de la imagen.
  - Variables de entorno reales (credenciales, clave JWT, URL de API) — nunca commiteadas.
- **Multi-stage build** de Docker para el frontend (etapa Node para construir → copiar estáticos a nginx).
- **nginx en prod**: servir los estáticos de React y hacer **proxy de `/api`** hacia PHP-FPM. Manejar el fallback de la SPA (rutas del cliente → `index.html`).
- **VPS**: elegir proveedor, acceso SSH, seguridad básica (usuario no-root, firewall, claves SSH, no password).
- **Dominio y DNS**: apuntar un dominio al servidor.
- **HTTPS**: certificados con Let's Encrypt (Certbot, o Caddy/Traefik que lo automatizan).
- **Gestión de secretos**: archivo `.env` en el servidor fuera del repo, o secretos de Docker.
- **Persistencia de datos** de MySQL en prod (volumen) y **copias de seguridad** (dump periódico).
- **Logs** y reinicio automático de contenedores (`restart: unless-stopped`).
- Proceso de **actualización/redeploy** (pull de código, rebuild, up).

## Preguntas de autoevaluación
1. ¿Por qué en prod no usas el dev-server de Vite ni bind mounts de código?
2. ¿Qué es un multi-stage build y qué problema resuelve para el frontend?
3. ¿Por qué las rutas del cliente (react-router) necesitan un fallback a `index.html` en nginx?
4. ¿Dónde y cómo guardas la clave secreta del JWT en producción?
5. ¿Qué pasa con los datos de MySQL si recreas el contenedor sin volumen? ¿Cómo haces backups?

## Ejercicios
- [ ] Crea un `Dockerfile` multi-stage para el frontend (build → estáticos en nginx).
- [ ] Crea un `docker-compose` de producción (sin bind mounts, con `restart`, con env reales).
- [ ] Configura nginx para servir la SPA + proxy `/api` + fallback a index.html.
- [ ] Aprovisiona un VPS, asegúralo (SSH por clave, firewall, usuario no-root).
- [ ] Apunta un dominio al servidor y configura HTTPS con Let's Encrypt.
- [ ] Despliega, verifica que todo funciona en el dominio con HTTPS.
- [ ] Configura un backup automático (dump de MySQL programado).
- [ ] Documenta tu proceso de redeploy paso a paso.

## Criterio de dominio
La plataforma es accesible públicamente por HTTPS en tu dominio, los datos persisten, tienes backups, y puedes redeployar una nueva versión con un proceso repetible y documentado.

## Checklist
- [ ] Frontend con multi-stage build
- [ ] docker-compose de producción
- [ ] nginx: estáticos + proxy /api + fallback SPA
- [ ] VPS asegurado (SSH, firewall)
- [ ] Dominio + HTTPS funcionando
- [ ] Secretos por variables de entorno en el servidor
- [ ] Backups automáticos de MySQL
- [ ] Proceso de redeploy documentado

## Recursos
- Docker — [multi-stage builds](https://docs.docker.com/build/building/multi-stage/).
- nginx — [guía de inicio](https://nginx.org/en/docs/beginners_guide.html) y [proxy inverso](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/).
- [Let's Encrypt](https://letsencrypt.org/) / [Certbot](https://certbot.eff.org/) (o [Caddy](https://caddyserver.com/) para HTTPS automático).
- [Initial Server Setup (SSH, firewall, usuario no-root)](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-22-04) — hardening básico.

## Enlaces internos
- Anterior: [cap 09b — Testing y calidad](09b-testing-y-calidad.md) · Siguiente: [cap 11 — Recursos y glosario](11-recursos-y-glosario.md)
- Relacionados: [cap 02 — Docker dev](02-docker-entorno-dev.md), [cap 06 — Secretos/JWT](06-autenticacion.md), [cap 01 — Monorepo](01-fundamentos-y-arquitectura.md)
- Índice: [00 — Índice](00-INDICE.md)
