---
tipo: patrón
dominio: multiagente
parent: "[[Multiagentes]]"
estado: por-ver
prereqs: ["[[Agente individual vs workflow vs multiagente]]"]
se_evalua_con: ["[[Multi-agent evals]]"]
contrasta_con: ["[[Router handoff]]"]
fuentes: []
bloque: "02 · Patrones alternativos"
orden: 220
---
# Orquestador / workers

> **En una frase:** un agente planifica y reparte, varios agentes especializados ejecutan en paralelo, y el orquestador junta los resultados. Divide y conquista, con LLMs.

## Diagrama
```mermaid
flowchart TB
  t[Tarea] --> o["Orquestador<br/>descompone en sub-tareas"]
  o --> w1[Worker: investigar]
  o --> w2[Worker: código]
  o --> w3[Worker: revisar]
  w1 & w2 & w3 --> o2["Orquestador<br/>sintetiza, ¿falta algo?"]
  o2 -->|falta| o
  o2 -->|listo| out[Resultado]
```

## Cuándo sí / cuándo no
| Usalo cuando | Evitalo cuando |
|---|---|
| La tarea se parte en pedazos independientes | Un solo agente con buenas tools lo resuelve |
| Cada pedazo necesita contexto o tools distintos | Los pedazos comparten mucho estado y se pisan |
| Podés pagar N llamadas en paralelo | Latencia y costo son la restricción |

## Decisiones de diseño
- **Qué ve cada worker**: solo su sub-tarea y lo mínimo de contexto. Pasarle todo es el error más común.
- **Formato de retorno**: estructurado (`resultado`, `confianza`, `fuentes`) para que el orquestador decida sin releer todo.
- **Fallas**: un worker falla → reintentar solo ese, no todo el plan.

## Se conecta con
Contrasta con [[Router handoff]]: el router elige *uno*, el orquestador coordina *varios*. Se mide con [[Multi-agent evals]].

[[Subagents]] desarrolla el contrato de delegación; [[Deep Agents]] ofrece una implementación integrada.
