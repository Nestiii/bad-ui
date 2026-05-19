# prompts.md — API SistemaReservas

Secuencia de prompts usados para modelar el contrato OpenAPI (`openapi.yaml`).
Conversación hecha con **Claude Code (Opus 4.7)**, como continuación del proyecto
`bad-ui`: la consigna pedía modelar una API en formato OpenAPI, y la armamos
sobre el dominio del formulario de reservas construido en la primera parte.

---

## 1 — Briefing de la consigna

> *(pegué la diapositiva "Lo que el yaml tiene que tener": 5 requisitos —tres
> methods mínimo (GET/POST/DELETE), jerarquía de recursos visible en el path,
> al menos una respuesta de error 400/404 documentada, schemas tipados con
> `type`/`required`/`format`, e iteración en vivo agregando un endpoint sin
> reescribir todo. Y el "lo que NO entra": implementación, base de datos,
> auth, deploy.)*

**Anotación:** Pasé los 5 requisitos y el alcance en un solo mensaje antes de
pedir nada de YAML. Así el modelo encuadró el entregable como **un contrato**,
no como una API funcional, y evité que se fuera a proponer base de datos o
lógica de negocio que la consigna explícitamente excluye.

---

## 2 — Elección de dominio

> ¿Qué podríamos modelar usando el formulario de bad-ui?

**Anotación:** El modelo identificó que el formulario horrible es, por debajo,
un **sistema de reserva de turnos**, y propuso modelar el backend limpio que
ese form alimentaría. Elegí esa idea porque reusar el dominio de la primera
parte le da continuidad al trabajo. La decisión concreta fue la **jerarquía**:
`recursos → turnos` (un recurso reservable —consultorio, cancha, profesional—
con su agenda de turnos). Eso cubre el Requisito 2 de forma natural.

---

## 3 — Primera versión del contrato

> Generá el `openapi.yaml`: `GET`/`POST`/`DELETE` sobre turnos, anidados dentro
> de recursos.

**Anotación:** Versión base. Salieron los paths `/recursos`,
`/recursos/{recursoId}` y `/recursos/{recursoId}/turnos/{turnoId}` con los tres
métodos (Requisito 1) y el anidamiento visible (Requisito 2). Pedí desde el
arranque que las respuestas de error y los parámetros vivieran en `components`,
para no tener que refactorizar después.

---

## 4 — Iteración: conectar los errores con el formulario

> El `400` del `POST` debería reusar el código `ERROR_47B`, que es el mismo que
> tira el formulario bad-ui cuando faltan campos.

**Anotación:** Primer ajuste fino. En vez de un error genérico, hice que el
contrato y el artefacto de la primera parte hablen el mismo idioma: el `ERROR_47B`
del `index.html` aparece como ejemplo en la respuesta `400`. Pequeño, pero deja
el entregable cohesionado. Aproveché para sumar también un `409` (turno ya
ocupado), que el formulario no contempla pero una API real sí necesita.

---

## 5 — Iteración: separar entrada de salida

> Separá lo que el cliente manda de lo que la API devuelve: el `id`, el
> `codigoConfirmacion` y `creadoEn` no deberían poder enviarse en el `POST`.

**Anotación:** Acá se nota la diferencia entre "tipar" y "tipar bien". Quedaron
dos schemas: `NuevoTurno` (lo que entra) y `Turno` (lo que sale), y los campos
generados por el servidor marcados como `readOnly`. Esto refuerza el Requisito 4
—`type`, `required` y `format` (`uuid`, `date-time`, `email`, `int32`)— mostrando
que los schemas modelan un rol, no solo una forma.

---

## 6 — Iteración en vivo: agregar un endpoint

> Agregá `GET /recursos/{recursoId}/disponibilidad` para consultar slots libres,
> sin reescribir el resto del archivo.

**Anotación:** Esta es la prueba directa del **Requisito 5**. El endpoint nuevo
no obligó a tocar nada de lo anterior: reutilizó el parámetro `RecursoId` y la
respuesta `NoEncontrado` que ya estaban en `components`, y solo sumó un path y
un schema (`SlotDisponibilidad`). El cambio fue aditivo — exactamente lo que un
buen contrato debería permitir.

---

## Decisión de prompt que enseñó algo

La más útil fue la **#3**: pedir desde el primer prompt que parámetros y
respuestas vivieran en `components`, antes de tener un solo endpoint extra.

Es la decisión más aburrida de la cadena y la que más rindió. Cuando llegó el
Requisito 5 (agregar un endpoint en vivo), no hubo refactor: el `GET
/disponibilidad` enganchó con piezas que ya existían. Si hubiera dejado los
parámetros y errores escritos inline en cada path —que es lo que un LLM tiende
a hacer si no se lo pedís—, "agregar un endpoint sin reescribir todo" habría
implicado, justamente, reescribir bastante.

**La estructura reutilizable no se nota mientras escribís el primer endpoint;
se nota cuando agregás el cuarto. Conviene pedirla antes de necesitarla.**
