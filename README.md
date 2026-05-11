# Bad UI — Selector de fecha y hora

Artefacto de la consigna **bad UI**. La consigna pedía construir la peor versión posible de algo común — funcional, pero diseñada para frustrar al usuario.

## ¿Qué es?

Un selector de fecha y hora para "reservar un turno". Cumple su función — al final podés llegar a un timestamp válido — pero cada uno de los 7 campos está intencionalmente roto de una manera distinta.

Horrores incluidos:

1. **Año** — flechas invertidas (▲ decrementa, ▼ incrementa).
2. **Mes** — 12 botones numerados 1–12, con mapeo a meses reales aleatorio en cada carga. Tooltip con el nombre real aparece después de 2 segundos de hover.
3. **Día** — calendario con encabezados de columna (L M X J V S D) mezclados al azar.
4. **Hora** — slider invertido (deslizar a la derecha = hora menor) y el dígito en pantalla tiembla constantemente.
5. **Minutos** — dos inputs: decenas admite solo {0, 1, 3, 5}, unidades admite solo múltiplos de 7. Solo se pueden alcanzar 8 valores totales (00, 07, 10, 17, 30, 37, 50, 57). Reservar a las :15 es imposible.
6. **AM/PM** — hay que mantener apretado el botón 3 segundos para alternar.
7. **Confirmar** — el botón se escapa del cursor con 60% de probabilidad al hover.

Un cuadro tipo terminal arriba muestra el estado actual en numerales romanos y epoch hexadecimal, porque sí.

## Quién lo construyó

Facundo Rivas — usando Claude Code (Opus 4.7) en una conversación corta. La consigna original pedía ChatGPT Canvas; usé Claude Code en su lugar.

## Cómo correrlo

Doble click en `index.html`. Sin build, sin dependencias, sin servidor.

## Qué funcionó

- **Single file fácil de mantener.** Una sola conversación produjo un archivo legible (~440 líneas) con CSS y JS inline. Volver atrás a tocar un horror específico fue rápido.
- **Una restricción por campo.** En vez de meter dos horrores en cada input, cada campo tiene un patrón claro y aislado. Eso hace el código más simple y los horrores más reconocibles para el usuario.
- **Acotar el flujo antes de codear.** Definir primero el artefacto (selector de fecha/hora) y la intensidad (maximalismo) antes de escribir HTML evitó que la conversación se dispersara entre ideas.
- **Mantener accesibilidad mínima.** Todos los campos usan inputs nativos (`button`, `range`, `number`). La frustración viene de la lógica, no de hacks que rompen el tab o el teclado — eso hubiera sido bug, no bad UI.

## Qué no funcionó (o quedó flojo)

- **El slider invertido es demasiado obvio.** Después de un par de movimientos el patrón se nota y deja de frustrar. Un horror más sutil ahí (sensibilidad logarítmica, o saltos no lineales) hubiera durado más como tortura.
- **Pedirle a un LLM "que sea malo a propósito" requiere recordárselo.** El modelo tiende a "ayudar" — agregar tooltips útiles, validaciones claras, mensajes de error legibles. Hay que estar atento a esos defaults helpful que diluyen el bad UI sin que uno los pida.
- **Faltó probar en móvil.** El `mouseenter` del botón confirmar no existe en touch, así que en celular el horror #7 desaparece. Habría que sustituirlo por algo equivalente en touch (un drag absurdo, por ejemplo).
