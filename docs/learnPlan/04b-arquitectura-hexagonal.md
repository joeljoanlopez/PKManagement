# Cap 04b — Arquitectura hexagonal (puertos y adaptadores)

## Objetivo
Entender y aplicar arquitectura hexagonal en el backend PHP nativo: aislar el dominio de la infraestructura mediante puertos y adaptadores. Sentar el patrón antes de construir la API y las features.

## Por qué
Vienes de Laravel, donde el "dominio" suele mezclarse con Eloquent, controladores y el framework. La hexagonal invierte eso: **tu lógica de negocio no conoce ni HTTP ni MySQL**. Al hacerlo sin framework, entiendes de verdad por qué la dependencia debe apuntar hacia el dominio y no al revés. Este capítulo define el patrón; los capítulos [05](05-api-router-capas.md), [06](06-autenticacion.md) y [09](09-features-parking.md) lo aplican.

## Conceptos a dominar
- **Regla de dependencia**: las capas externas dependen de las internas, nunca al revés. El dominio no importa nada de infraestructura.
- **Dominio (núcleo)**: entidades, objetos de valor y reglas de negocio puras. Sin PDO, sin `$_SERVER`, sin JSON.
- **Puertos**:
  - *Driving/entrada* (primary ports): interfaces que expone la aplicación (casos de uso) para que el mundo exterior la invoque.
  - *Driven/salida* (secondary ports): interfaces que la aplicación **necesita** de la infraestructura (ej: `SubscriberRepository` como **interfaz**, no implementación).
- **Adaptadores**:
  - *Driving/entrada*: el que traduce el mundo exterior a llamadas de la app. En tu caso el **controlador HTTP + router** ([cap 05](05-api-router-capas.md)) es un adaptador de entrada. Podrías tener otro (CLI) sin tocar el dominio.
  - *Driven/salida*: implementación concreta de un puerto de salida. Ej: `PdoSubscriberRepository` que implementa la interfaz `SubscriberRepository` usando PDO ([cap 04](04-mysql-pdo-modelado.md)).
- **Capa de aplicación / casos de uso** (application services): orquestan el dominio y los puertos de salida. Un caso de uso = una acción del negocio ("Registrar abonado", "Asignar plaza").
- **Inversión de dependencias (DIP)**: el dominio define la interfaz del repositorio; la infraestructura la implementa. El caso de uso recibe la interfaz (no la clase concreta).
- **Inyección de dependencias manual**: sin el contenedor de Laravel. Un "composition root" (punto de arranque) donde construyes y cableas las clases concretas.
- **DTOs**: objetos para mover datos entre capas sin filtrar entidades del dominio hacia HTTP.
- **Mapeo de errores**: el dominio lanza excepciones de negocio; el adaptador de entrada las traduce a códigos HTTP (el dominio no sabe qué es un 404).

## Estructura de carpetas sugerida (backend)
Guía, no dogma. Organiza por capas (o por módulo/bounded context + capas):
```
backend/src/
  Domain/           ← entidades, value objects, excepciones de negocio, INTERFACES de puertos de salida
  Application/      ← casos de uso (application services), DTOs
  Infrastructure/
    Http/           ← adaptador de entrada: router, controladores, middleware
    Persistence/    ← adaptador de salida: implementaciones PDO de los repositorios
  Shared/           ← utilidades transversales
```
> Piensa: si borraras `Infrastructure/`, `Domain/` y `Application/` deben seguir compilando y teniendo sentido.

## Preguntas de autoevaluación
1. ¿Por qué la interfaz del repositorio vive en el dominio y la implementación PDO en infraestructura?
2. ¿Qué es un adaptador de entrada y cuál será el tuyo? ¿Podrías añadir otro sin tocar el dominio?
3. ¿En qué se diferencia un caso de uso de un controlador?
4. Si el dominio lanza `SubscriberNotFound`, ¿quién y dónde la convierte en un HTTP 404?
5. Sin el contenedor de Laravel, ¿dónde y cómo cableas las dependencias concretas?
6. ¿Por qué un DTO en vez de devolver la entidad directamente al cliente?

## Ejercicios
- [ ] Define la estructura de carpetas del backend según capas.
- [ ] Modela una entidad de dominio (ej: `Subscriber`) pura, sin dependencias de infraestructura.
- [ ] Declara el puerto de salida `SubscriberRepository` como **interfaz** en el dominio.
- [ ] Implementa `PdoSubscriberRepository` en infraestructura (usa lo del cap 04).
- [ ] Escribe un caso de uso "Registrar abonado" que dependa de la interfaz, no de PDO.
- [ ] Crea el composition root: cablea el controlador → caso de uso → repositorio PDO a mano.
- [ ] Demuestra el aislamiento: escribe una implementación en memoria del repositorio y usa el mismo caso de uso sin cambiar el dominio.

## Criterio de dominio
Puedes cambiar la implementación de persistencia (PDO ↔ en memoria) sin tocar dominio ni casos de uso, y explicar por qué las dependencias apuntan hacia adentro. El dominio no contiene ni una línea de PDO ni de HTTP.

## Advertencia práctica
No sobre-ingenierices. Hexagonal aporta cuando hay lógica de negocio real (suscripciones, pagos, reglas de plazas). Para CRUD trivial, mantén las capas pero sin ceremonia excesiva. Aprende el patrón, aplícalo con criterio.

## Checklist
- [ ] Estructura de carpetas por capas
- [ ] Entidad de dominio pura
- [ ] Puerto de salida (interfaz) en dominio
- [ ] Adaptador de salida PDO
- [ ] Al menos un caso de uso desacoplado
- [ ] Composition root cableando a mano
- [ ] Repositorio en memoria demostrando el aislamiento

## Recursos
- Alistair Cockburn — [Hexagonal Architecture (Ports and Adapters)](https://alistair.cockburn.us/hexagonal-architecture/).
- Robert C. Martin — [The Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) (regla de dependencia).
- Ejemplos de hexagonal en PHP (busca repos de referencia; léelos, no los copies).

## Enlaces internos
- Anterior: [cap 04 — MySQL + PDO](04-mysql-pdo-modelado.md) · Siguiente: [cap 05 — API router y capas](05-api-router-capas.md)
- Se aplica en: [cap 05 — API (adaptador de entrada)](05-api-router-capas.md), [cap 06 — Autenticación](06-autenticacion.md), [cap 09 — Features](09-features-parking.md), [cap 09b — Testing (dominio aislado)](09b-testing-y-calidad.md)
- Depende de: [cap 04 — MySQL + PDO](04-mysql-pdo-modelado.md)
- Índice: [00 — Índice](00-INDICE.md)
