---
tipo: eval
dominio: evals
estado: por-ver
parent: "[[Evals]]"
prereqs: ["[[Graders]]", "[[Golden dataset]]"]
se_evalua_con: []
contrasta_con: []
fuentes:
  - https://arxiv.org/abs/2306.05685
  - https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents
bloque: "01 · Casos y criterios"
orden: 140
---
# Calibración de evaluadores

> **En una frase:** antes de confiar en el puntaje del juez, comprobá qué errores comete frente a una referencia humana.

## Un procedimiento corto
1. **Rúbrica:** definí qué aprueba, qué falla y cuándo falta evidencia. Usá código para montos, permisos y estados verificables.
2. **Muestra humana:** etiquetá casos correctos, incorrectos y ambiguos; resolvé desacuerdos entre revisores antes de fijar la referencia.
3. **Comparación:** aplicá el juez a los mismos casos, sin mostrarle las etiquetas humanas. Revisá falsos aprobados y falsos rechazados por tipo de tarea.
4. **Ajuste y validación:** mejorá la rúbrica con una muestra y comprobá el resultado en otra reservada. Versioná juez, prompt y referencias. [Diseño de evals](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents).

## Ejemplo ficticio
| Referencia humana | Juez aprueba | Juez rechaza |
|---|---:|---:|
| 10 respuestas correctas | 9 | 1 |
| 10 respuestas incorrectas | 3 | 7 |

El acuerdo es **16/20 = 80%**, pero el juez aprueba **3/10 = 30% de las incorrectas**. El promedio oculta un problema si esos fallos son críticos.

## Evitar engaños
- En comparaciones A/B, intercambiá el orden y ocultá el nombre del modelo. Probá respuestas largas pero incorrectas: posición y verbosidad pueden sesgar al juez. [Estudio de jueces LLM](https://arxiv.org/abs/2306.05685).
- Repetí casos para observar variabilidad; para comparar sistemas usá los mismos casos, reportá tamaño de muestra e incertidumbre. Una corrida no basta.
- Conservá “no evaluable” como categoría separada, con revisión humana.

## Practicá
> [!question]- ¿Aceptarías 95 % de acuerdo si todos los desacuerdos son filtraciones de datos?
> No. El acuerdo pesa igual todos los errores, y una filtración es un fallo crítico. Mirá de qué lado están: si el juez aprueba filtraciones que los humanos rechazaron, deja pasar justo lo que más importa. Reportá los errores por tipo y severidad, exigí cero falsos aprobados en esa categoría y verificá permisos y datos de cliente con código, sin depender del juez. Es el mismo problema del ejemplo: 80 % de acuerdo escondía 30 % de incorrectas aprobadas.

## Se conecta con
[[Regresiones y CI]] · [[Caso práctico - Asistente de soporte]]
