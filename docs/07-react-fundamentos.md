# Cap 07 — React: fundamentos

## Objetivo
Dominar los fundamentos de React con Vite antes de tocar tu API: componentes, estado, efectos, formularios.

## Por qué
Es tu área más nueva. No saltes a librerías (router, estado global) hasta que los fundamentos sean sólidos, o construirás sobre arena. Trabaja primero con datos falsos en memoria.

## Conceptos a dominar
- **Entorno**: crear proyecto con Vite, estructura, `npm run dev`, hot reload.
- **JSX**: qué es y cómo se diferencia del HTML.
- **Componentes** funcionales y composición.
- **Props**: pasar datos de padre a hijo; flujo de datos unidireccional.
- **Estado con `useState`**: por qué el estado dispara re-render; inmutabilidad (no mutar arrays/objetos, crear nuevos).
- **Renderizado de listas**: `.map()` y la importancia de `key`.
- **Renderizado condicional**.
- **Eventos**: manejar clicks, inputs.
- **Formularios controlados**: el estado como fuente de verdad del input.
- **`useEffect`**: efectos secundarios, array de dependencias, función de limpieza. Entender cuándo se ejecuta y errores comunes (bucles infinitos).
- **Levantar el estado** (lifting state up) y por qué a veces se comparte en el ancestro común.
- Reglas de los **Hooks**.

### Estilado con Tailwind CSS
- Qué es **utility-first CSS** y en qué se diferencia de escribir CSS tradicional o usar componentes prefabricados.
- Instalación e integración de Tailwind con Vite.
- Clases de utilidad: layout (flex, grid), espaciado, tipografía, colores, estados (`hover:`, `focus:`), responsive (`sm:`, `md:`, `lg:`).
- Cómo evitar el "muro de clases": extraer componentes de React reutilizables en vez de duplicar clases.
- Tema y personalización básica (colores, fuentes) en la config de Tailwind.

### Iconos con Lucide
- Instalar `lucide-react` y usar iconos como componentes.
- Props comunes: `size`, `color`, `strokeWidth`; estilar con clases de Tailwind.
- Buenas prácticas: iconos con significado accesible (`aria-label` cuando el icono es la única señal).

## Preguntas de autoevaluación
1. ¿Por qué no debes mutar directamente un array de estado con `push`? ¿Qué haces en su lugar?
2. ¿Qué causa que un componente se vuelva a renderizar?
3. ¿Para qué sirve `key` en una lista y qué pasa si usas el índice como key mal?
4. ¿Cuándo se ejecuta `useEffect` y para qué sirve el array de dependencias?
5. ¿Qué es un formulario controlado y por qué el input lee su valor del estado?

## Ejercicios (con datos falsos, sin API todavía)
- [ ] Crea el proyecto con Vite y familiarízate con la estructura.
- [ ] Integra Tailwind con Vite y verifica que las clases de utilidad funcionan.
- [ ] Instala `lucide-react` y coloca algún icono en la UI.
- [ ] Construye un componente que muestre una lista de abonados desde un array en memoria, estilado con Tailwind.
- [ ] Añade un formulario controlado para "crear" un abonado (que se añada al array en estado).
- [ ] Implementa editar y eliminar sobre ese array en memoria.
- [ ] Añade filtro/búsqueda por nombre en el cliente.
- [ ] Extrae al menos un componente reutilizable (ej: botón, input) para no repetir clases de Tailwind.
- [ ] Provoca un bucle infinito con `useEffect` a propósito, entiéndelo y arréglalo.

## Criterio de dominio
Puedes construir una pantalla CRUD completa funcionando solo con estado en memoria, sin mirar tutoriales, y explicar por qué se re-renderiza cada vez.

## Checklist
- [ ] Proyecto Vite corriendo
- [ ] Tailwind integrado y funcionando
- [ ] Lucide instalado, iconos en uso
- [ ] Lista renderizada con .map + keys
- [ ] Formulario controlado
- [ ] CRUD en memoria (crear/editar/borrar)
- [ ] Filtro en cliente
- [ ] Componentes reutilizables extraídos
- [ ] Entiendo useEffect y sus dependencias

## Recursos
- react.dev — [Learn React](https://react.dev/learn) (la mejor fuente, empieza aquí).
- Vite — [guía de inicio](https://vite.dev/guide/).
- react.dev — [Rules of Hooks](https://react.dev/reference/rules/rules-of-hooks), [You Might Not Need an Effect](https://react.dev/learn/you-might-not-need-an-effect).
- Tailwind CSS — [instalación con Vite](https://tailwindcss.com/docs/installation/using-vite) y [documentación](https://tailwindcss.com/docs).
- [lucide.dev](https://lucide.dev/) — iconos y [`lucide-react`](https://lucide.dev/guide/packages/lucide-react).

## Enlaces internos
- Anterior: [cap 06 — Autenticación](06-autenticacion.md) · Siguiente: [cap 08 — React consumo de API](08-react-consumo-api.md)
- Relacionados: [cap 09b — Testing FE](09b-testing-y-calidad.md)
- Índice: [00 — Índice](00-INDICE.md)
