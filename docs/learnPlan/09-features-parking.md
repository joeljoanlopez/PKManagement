# Cap 09 — Construcción de features (parking de abonados)

## Objetivo
Construir la plataforma completa por **vertical slices**: cada feature de punta a punta (BD → API → UI) antes de pasar a la siguiente.

## Por qué
Construir por capas horizontales (toda la BD, luego toda la API, luego toda la UI) retrasa el feedback y esconde errores de diseño. Un slice vertical terminado te da algo funcional pronto y valida la arquitectura completa en cada paso.

## Método por cada slice
Para cada feature, repite este ciclo:
1. **Modelo**: ajusta/crea tablas necesarias.
2. **API**: endpoints (Controller → Repository), validación, códigos HTTP.
3. **Probar con curl/Postman** ANTES de la UI.
4. **UI**: pantalla React conectada a esos endpoints.
5. **Commit** cuando el slice funcione de extremo a extremo.

## Orden recomendado de slices

### Slice 1 — Operarios y autenticación
- [ ] Ya cubierto en [cap 05](05-api-router-capas.md), [cap 06](06-autenticacion.md), [cap 08](08-react-consumo-api.md). Debe estar sólido antes de seguir.

### Slice 2 — Abonados (núcleo)
- [ ] CRUD completo: listar (con búsqueda/paginación), ver, crear, editar, cambiar estado (activo/suspendido/cancelado), borrar.
- [ ] Validaciones: documento único, formato de email/teléfono.

### Slice 3 — Vehículos
- [ ] Un abonado tiene N vehículos. CRUD de vehículos asociados a un abonado.
- [ ] En la UI: gestionar vehículos desde la ficha del abonado.

### Slice 4 — Plazas de parking
- [ ] CRUD de plazas (código, planta, estado ocupada/libre).
- [ ] Vista del estado del parking (ocupación).

### Slice 5 — Suscripciones (abonos mensuales)
- [ ] Asignar un abono a un abonado, con plaza, precio mensual y fechas.
- [ ] Reglas de negocio: no asignar una plaza ya ocupada; controlar fechas de inicio/fin.
- [ ] Usa transacción si creas suscripción + primer pago juntos.

### Slice 6 — Pagos
- [ ] Registrar pagos por periodo (mes). Marcar pagado/pendiente.
- [ ] Listado de pagos por abonado y por periodo.
- [ ] Detectar impagos del mes en curso.

### Slice 7 — Dashboard
- [ ] Métricas: plazas ocupadas/libres, abonados activos, ingresos del mes, impagos pendientes.
- [ ] Consultas SQL de agregación (COUNT, SUM, GROUP BY, JOINs).

## Conceptos que reforzarás aquí
- Relaciones entre entidades reflejadas en API y UI.
- Reglas de negocio en el backend (nunca confíes solo en validación del cliente).
- Paginación y filtrado en servidor.
- Consultas con JOIN y agregaciones para el dashboard.
- UX de formularios complejos y relaciones.

## Criterio de dominio
Un operario puede: dar de alta un abonado con su vehículo, asignarle una plaza y un abono mensual, registrar sus pagos y ver el estado general del parking en el dashboard. Todo con auth, validación en backend y datos persistidos.

## Checklist global de features
- [ ] Slice 2 — Abonados
- [ ] Slice 3 — Vehículos
- [ ] Slice 4 — Plazas
- [ ] Slice 5 — Suscripciones
- [ ] Slice 6 — Pagos
- [ ] Slice 7 — Dashboard
- [ ] MVP completo funcionando en local

## Recursos
- Reutiliza lo aprendido en [cap 04](04-mysql-pdo-modelado.md), [cap 05](05-api-router-capas.md), [cap 08](08-react-consumo-api.md).
- MySQL — [JOINs](https://dev.mysql.com/doc/refman/8.0/en/join.html) y [funciones de agregación](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html) para el dashboard.

## Enlaces internos
- Anterior: [cap 08 — React consumo de API](08-react-consumo-api.md) · Siguiente: [cap 09b — Testing y calidad](09b-testing-y-calidad.md)
- Aplica: [cap 04 — Modelado](04-mysql-pdo-modelado.md), [cap 04b — Hexagonal](04b-arquitectura-hexagonal.md), [cap 05 — API](05-api-router-capas.md), [cap 06 — Auth](06-autenticacion.md), [cap 08 — React](08-react-consumo-api.md)
- Índice: [00 — Índice](00-INDICE.md)
