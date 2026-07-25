# Cap 04 — MySQL + PDO + modelado de datos

## Objetivo
Diseñar el esquema de la base de datos del parking y acceder a ella de forma segura con PDO (tu "Eloquent a mano").

## Por qué
En Laravel usabas migraciones y Eloquent. Aquí escribes SQL y usas PDO directamente. Entender esto te hará mejor con cualquier ORM después, porque sabrás qué genera por debajo.

## Conceptos a dominar
### Modelado
- Diseño relacional: tablas, claves primarias, **claves foráneas**, relaciones 1:N y N:M.
- Tipos de datos MySQL apropiados (VARCHAR vs TEXT, DECIMAL para dinero —nunca FLOAT—, ENUM, DATE/TIMESTAMP).
- **Normalización** básica (evitar duplicar datos).
- Índices: qué son, cuándo ayudan (columnas de búsqueda y FKs), coste de escritura.
- Restricciones: `UNIQUE`, `NOT NULL`, `ON DELETE CASCADE / RESTRICT`.

### Acceso con PDO
- Conexión PDO (DSN, usuario, password, opciones: modo de error en excepción, fetch mode por defecto).
- **Prepared statements** y por qué previenen inyección SQL (esto es innegociable).
- `bindValue` vs pasar array a `execute`.
- Recuperar datos: `fetch`, `fetchAll`, fetch modes.
- `lastInsertId`, `rowCount`.
- **Transacciones**: `beginTransaction / commit / rollBack` (ej: crear suscripción + su primer pago juntos).

## Preguntas de autoevaluación
1. ¿Por qué se usa DECIMAL y no FLOAT para importes de dinero?
2. ¿Qué es exactamente una inyección SQL y cómo la elimina un prepared statement?
3. ¿Qué pasa con los vehículos de un abonado si borras el abonado? ¿Cómo lo controlas con la FK?
4. ¿Cuándo usarías una transacción en este proyecto? Da un ejemplo concreto.
5. ¿Por qué conviene configurar PDO para que lance excepciones en vez de fallar en silencio?

## Ejercicios
- [ ] Diseña el esquema completo: usuarios (operarios), abonados, vehículos, plazas, suscripciones, pagos. Define tipos, claves y relaciones.
- [ ] Escribe los `CREATE TABLE` en `.sql` versionados y cárgalos vía `docker-entrypoint-initdb.d`.
- [ ] Dibuja el diagrama entidad-relación (ERD).
- [ ] Crea la clase de conexión PDO leyendo credenciales de variables de entorno.
- [ ] Escribe una consulta con prepared statement que busque abonados por nombre parcial.
- [ ] Implementa una operación con transacción (crea suscripción + primer pago; si algo falla, rollback).

## Criterio de dominio
Tu esquema soporta todos los casos de uso del parking sin duplicar datos, y accedes a él con consultas parametrizadas seguras. Puedes explicar por qué ninguna de tus consultas es vulnerable a inyección.

## Checklist
- [ ] Esquema completo diseñado
- [ ] ERD dibujado
- [ ] Scripts .sql cargando en Docker
- [ ] Conexión PDO por entorno
- [ ] Consultas con prepared statements
- [ ] Ejemplo de transacción funcionando

## Recursos
- Manual de PHP: [PDO](https://www.php.net/manual/en/book.pdo.php), [prepared statements](https://www.php.net/manual/en/pdo.prepared-statements.php), [transacciones](https://www.php.net/manual/en/pdo.transactions.php).
- MySQL: [tipos de datos](https://dev.mysql.com/doc/refman/8.0/en/data-types.html), [claves foráneas](https://dev.mysql.com/doc/refman/8.0/en/create-table-foreign-keys.html), [índices](https://dev.mysql.com/doc/refman/8.0/en/mysql-indexes.html).
- [Normalización de bases de datos](https://en.wikipedia.org/wiki/Database_normalization) (1FN, 2FN, 3FN).

## Enlaces internos
- Anterior: [cap 03 — PHP nativo](03-php-nativo-fundamentos.md) · Siguiente: [cap 04b — Arquitectura hexagonal](04b-arquitectura-hexagonal.md)
- Relacionados: [cap 05 — API router y capas](05-api-router-capas.md), [cap 09 — Features del parking](09-features-parking.md), [cap 09b — Testing (integración BD)](09b-testing-y-calidad.md)
- Índice: [00 — Índice](00-INDICE.md)
