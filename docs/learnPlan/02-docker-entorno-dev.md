# Cap 02 — Docker: entorno de desarrollo

## Objetivo
Levantar con un solo comando (`docker compose up`) tres servicios —nginx, PHP-FPM, MySQL— comunicados entre sí, y servir una respuesta JSON de prueba desde PHP.

## Por qué
Tienes nociones medias de Docker pero nunca montaste un servicio así. Este entorno es la base de todo: si no está sólido, cada capítulo posterior duele. Hazlo bien una vez.

## Conceptos a dominar
- **Imagen vs contenedor**: plantilla vs instancia en ejecución.
- **Dockerfile**: cómo construir una imagen propia (partir de `php:8.3-fpm`, instalar extensiones como `pdo_mysql`).
- **docker-compose.yml**: definir varios servicios, sus imágenes/builds, puertos, dependencias.
- **Volumes**:
  - *Bind mount* (código del host dentro del contenedor → editas y ves cambios sin rebuild).
  - *Named volume* (persistir datos de MySQL entre reinicios).
- **Networks**: por defecto Compose crea una red; los contenedores se ven entre sí **por el nombre del servicio** (ej: PHP conecta a MySQL usando el host `mysql`, no `localhost`).
- **Puertos**: mapeo `host:contenedor`. Diferencia entre puerto interno y expuesto.
- **depends_on + healthcheck**: arrancar en orden y esperar a que MySQL esté listo.
- Cómo **nginx habla con PHP-FPM**: FastCGI por el puerto 9000. Nginx no "ejecuta" PHP; se lo pasa a FPM.
- **docker-entrypoint-initdb.d** de la imagen MySQL: ejecuta tus `.sql` al crear el contenedor por primera vez.
- Comandos clave: `up`, `down`, `build`, `logs -f`, `exec`, `ps`.

## Preguntas de autoevaluación
1. ¿Por qué desde el contenedor PHP conectas a la BD con host `mysql` y no `localhost`?
2. ¿Qué diferencia hay entre `docker compose down` y `docker compose down -v`? ¿Cuándo perderías los datos?
3. Si editas un archivo PHP, ¿por qué se refleja sin reconstruir la imagen? ¿Qué lo hace posible?
4. ¿Por qué nginx necesita `fastcgi_pass` apuntando a `php:9000`?
5. ¿Qué pasa si PHP intenta conectar a MySQL antes de que MySQL termine de arrancar? ¿Cómo lo evitas?

## Ejercicios
- [ ] Escribe tu `../../docker-compose.yml` con los 3 servicios (investiga cada clave que uses; no copies a ciegas).
- [ ] Escribe el `Dockerfile` del backend e instala las extensiones PHP necesarias.
- [ ] Configura el `nginx.conf` para servir `public/index.php` y pasar `.php` a FPM.
- [ ] Crea un `index.php` que devuelva `{"ok": true}` y compruébalo en el navegador.
- [ ] Entra al contenedor MySQL con `docker compose exec` y verifica que tu base de datos existe.
- [ ] Provoca a propósito un fallo (puerto ocupado, nombre de host mal) y lee los `logs` para diagnosticarlo.

## Criterio de dominio
`docker compose up --build` levanta todo, `localhost:8080` responde el JSON, y puedes explicar qué hace cada servicio y cómo se comunican.

## Checklist
- [ ] docker-compose.yml funcionando
- [ ] Dockerfile del backend
- [ ] nginx.conf sirviendo PHP
- [ ] Endpoint /ok devolviendo JSON
- [ ] MySQL persiste datos tras `down` + `up`
- [ ] Sé leer los logs para depurar

## Recursos
- [Docker Compose — documentación](https://docs.docker.com/compose/) y [referencia del formato del archivo](https://docs.docker.com/reference/compose-file/).
- Docker Hub: imágenes oficiales [`php`](https://hub.docker.com/_/php), [`mysql`](https://hub.docker.com/_/mysql), [`nginx`](https://hub.docker.com/_/nginx).
- nginx — [módulo FastCGI (`fastcgi_pass`)](https://nginx.org/en/docs/http/ngx_http_fastcgi_module.html) y [guía de inicio](https://nginx.org/en/docs/beginners_guide.html).

## Enlaces internos
- Anterior: [cap 01 — Fundamentos y arquitectura](01-fundamentos-y-arquitectura.md) · Siguiente: [cap 03 — PHP nativo](03-php-nativo-fundamentos.md)
- Relacionados: [cap 04 — MySQL + PDO](04-mysql-pdo-modelado.md), [cap 10 — Deploy](10-deploy.md)
- Índice: [00 — Índice](00-INDICE.md)
