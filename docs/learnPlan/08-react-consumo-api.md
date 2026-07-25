# Cap 08 — React: consumo de API y estado de servidor

## Objetivo
Conectar React con tu API PHP: peticiones HTTP, manejo de carga/errores, rutas, y sincronización de datos de servidor.

## Por qué
Aquí unes los dos mundos. El reto real no es "hacer un fetch", sino gestionar bien estados de carga, errores, cache y actualizaciones tras mutaciones.

## Conceptos a dominar
- **Peticiones HTTP** desde el navegador: `fetch` (o `axios`), `async/await`, enviar/recibir JSON, cabeceras.
- Estados de una petición: **cargando / éxito / error**, y cómo reflejarlos en la UI.
- **Variables de entorno en Vite** (`import.meta.env`) para la URL base de la API (distinta en dev y prod).
- **CORS desde el lado cliente**: entender el error cuando aparece (lo resuelves en el backend, cap 05).
- **Enrutado con `react-router-dom`**: páginas, rutas con parámetros, navegación, rutas protegidas (redirigir si no hay sesión).
- **Autenticación en el cliente**: guardar el token, enviarlo en cada petición (`Authorization: Bearer`), manejar expiración (401 → logout/redirect).
- **Estado de servidor con TanStack Query** (React Query): queries, mutations, invalidación de cache, refetch. Entiende por qué esto sustituye a mucho `useEffect` + `useState` manual.
- Organización: capa de "cliente API" reutilizable en vez de `fetch` esparcido por los componentes.
- Manejo de formularios más serios (validación en cliente; recuerda que la validación real vive en el backend).

### Estructura del frontend (separación de responsabilidades)
Aplica el mismo espíritu de la hexagonal al FE: aísla el acceso a datos de la UI. React no es un backend, así que sé pragmático, pero mantén fronteras claras.
- **Organización feature-based** (por dominio, no por tipo de archivo). Ejemplo:
```
frontend/src/
  features/
    subscribers/      ← componentes, hooks y llamadas de "abonados"
    vehicles/
    parking/
    auth/
  shared/
    api/              ← cliente HTTP (el "adaptador" hacia el backend): URL base, token, manejo de errores
    ui/               ← componentes reutilizables (botón, input, tabla, iconos)
    hooks/
  routes/             ← definición de rutas y layout
```
- El **cliente API** (`shared/api`) es tu única puerta al backend: si cambia la API, tocas un sitio. Los componentes no llaman a `fetch` directamente.
- Los **componentes** consumen datos vía hooks (de TanStack Query), no conocen detalles de HTTP.
- Reglas de negocio del frontend (formateo, cálculos de UI) separadas de la presentación.

## Preguntas de autoevaluación
1. ¿Qué tres estados mínimos debe contemplar toda petición y cómo los muestras?
2. ¿Por qué la URL de la API no debe estar hardcodeada y cómo la cambias entre dev y prod?
3. ¿Qué hace la "invalidación" de una query tras una mutación y por qué la necesitas?
4. ¿Cómo proteges una ruta para que solo entren usuarios autenticados?
5. Si el backend responde 401, ¿qué debería hacer el frontend?

## Ejercicios
- [ ] Define la estructura de carpetas feature-based del frontend.
- [ ] Crea una capa de cliente API (`shared/api`) que centralice la URL base, añada el token de auth y maneje errores en un solo sitio.
- [ ] Conecta la pantalla de abonados a tu API real (reemplaza los datos en memoria del [cap 07](07-react-fundamentos.md)), consumiendo datos vía hooks, sin `fetch` en los componentes.
- [ ] Muestra estados de carga y error de forma visible.
- [ ] Añade `react-router-dom`: páginas de login, listado y detalle.
- [ ] Implementa el login real contra tu endpoint y guarda el token.
- [ ] Protege rutas: sin token → redirige a login.
- [ ] Integra TanStack Query para listar y para las mutaciones (crear/editar/borrar) con invalidación.

## Criterio de dominio
Tu app hace login real, consume datos protegidos de tu API, refleja carga/errores, y tras crear/editar un abonado la lista se actualiza sola sin recargar la página.

## Checklist
- [ ] Estructura feature-based definida
- [ ] Cliente API centralizado (adaptador único) con token y manejo de errores
- [ ] Pantalla de abonados contra API real
- [ ] Estados de carga y error en UI
- [ ] react-router con rutas protegidas
- [ ] Login real guardando token
- [ ] TanStack Query con invalidación tras mutaciones

## Recursos
- react.dev — [fetching de datos](https://react.dev/learn/synchronizing-with-effects#fetching-data) y [You Might Not Need an Effect](https://react.dev/learn/you-might-not-need-an-effect).
- [TanStack Query](https://tanstack.com/query/latest) — queries, mutations, invalidation.
- [React Router](https://reactrouter.com/) — documentación oficial.
- Vite — [variables de entorno (`import.meta.env`)](https://vite.dev/guide/env-and-mode).
- MDN — [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API).

## Enlaces internos
- Anterior: [cap 07 — React fundamentos](07-react-fundamentos.md) · Siguiente: [cap 09 — Features del parking](09-features-parking.md)
- Consume: [cap 05 — API router](05-api-router-capas.md), [cap 06 — Autenticación](06-autenticacion.md). Arquitectura FE inspirada en [cap 04b — Hexagonal](04b-arquitectura-hexagonal.md). Se testea en [cap 09b](09b-testing-y-calidad.md)
- Índice: [00 — Índice](00-INDICE.md)
