# prompts.md

Secuencia de prompts usados para construir este artefacto. Conversación hecha con **Claude Code (Opus 4.7)**, no con ChatGPT Canvas como pedía la consigna original.

---

## 1 — Briefing completo de la actividad

> *(pegué el texto íntegro de la consigna: encuadre teórico, tema bad UI, referencia a r/badUIbattles, constraints técnicos —single file, sin dependencias, sin build tools—, y descripción del entregable —index.html, prompts.md, README.md—.)*

**Anotación:** Le pasé todo el briefing en un solo mensaje para que el modelo tuviera el contexto del género, los constraints y la forma del entregable antes de proponer cualquier cosa. Esto evitó tener que ir corrigiendo decisiones después.

---

## 2 — Modo de colaboración

> Construir el index.html acá.

**Anotación:** El modelo me ofreció cuatro modos de ayudar (brainstorm de ideas, scaffolding del repo, construir directo, o solo aclarar la consigna). Elegí construir directo. Es la decisión más importante de la conversación: si hubiera pedido brainstorm primero, el flujo hubiera tenido dos rondas más y posiblemente otro artefacto.

---

## 3 — Concepto y nivel de intensidad

> Selector de hora / fecha · Acumulación de horrores.

**Anotación:** Dos decisiones juntas — qué cosa común torturar, y cuánto. El modelo dio opciones de artefacto (form de login, calculadora, slider de volumen, selector de fecha/hora) y de intensidad (un gimmick pulido vs. maximalismo total). Elegí selector de fecha + maximalismo porque tiene muchos campos distintos donde meter un horror por cada uno — mejor demostración del rango de patrones bad UI que el género puede ofrecer.

---

## 4 — Cierre del entregable

> Generar readme y prompts, no importa que lo hicimos con claude code. Mi nombre es Facundo Rivas. Además agregá un ignore con el .idea por favor.

**Anotación:** Pedí los dos archivos de soporte del entregable y el `.gitignore`. Aclaré explícitamente que está bien mencionar Claude Code en lugar de ChatGPT — preferí dejar registro honesto del proceso antes que inventar una conversación de ChatGPT que no ocurrió.

---

## Decisión de prompt que enseñó algo

La más interesante fue la **#2**. Cuando el modelo ofreció cuatro modos distintos de colaborar **antes** de tocar código, me forzó a decidir qué quería realmente: ¿que el modelo piense por mí?, ¿que ejecute?, ¿que me ayude a hacerlo yo? Es una pregunta que normalmente no me hago al sentarme a usar un LLM, y la respuesta cambia toda la conversación.

Elegir "construir directo" hizo todo mucho más rápido pero también me sacó del loop creativo — los siete horrores específicos los eligió el modelo, yo solo aprobé el concepto general (fecha + maximalismo). La próxima vez probablemente elegiría "brainstorm primero" para tener más voz sobre los patrones, aceptando el costo de una conversación más larga. **Velocidad de ejecución y autoría de las decisiones son trade-off; un LLM rápido tiende a empujar la balanza hacia él.**
