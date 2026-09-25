---
tipo: mapa
parent: "[[Multiagentes]]"
dominio: multiagente
estado: por-ver
prereqs: ["[[Orquestador workers]]"]
se_evalua_con: ["[[Evaluación de subagentes]]"]
contrasta_con: []
fuentes:
  - https://docs.langchain.com/oss/javascript/langchain/multi-agent/subagents
  - https://docs.langchain.com/oss/javascript/deepagents/overview
bloque: "03 · Delegación y SDK"
orden: 310
---
# Subagents y Deep Agents

> **En una frase:** subagents es un patrón de delegación; Deep Agents es un SDK que lo integra con otras capacidades.

```mermaid
flowchart LR
  a[Delegar una subtarea] --> b[Patrón de subagents]
  b --> c[Componer con LangChain]
  b --> d[Usar Deep Agents]
  d --> e[Contexto, archivos y delegación integrados]
```

## Fichas breves
| Nota | Pregunta |
|---|---|
| [[Subagents]] | ¿Qué recibe, ejecuta y devuelve un subagente? |
| [[Deep Agents]] | ¿Qué capacidades trae el SDK? |

**Ejemplo:** revisar pólizas y antecedentes puede repartirse entre subagentes. Deep Agents es una opción para construir esa solución.

> [!TIP] Para recordar
> Podés usar subagentes sin Deep Agents. Elegí según las capacidades y el control que necesites.

## Evaluación en Evals
[[Evaluación de subagentes]] → [[Decisión de delegar]] → [[Casos de eval de subagentes]] → [[Evals de Deep Agents]].

## Estado de los nodos
```dataview
TABLE WITHOUT ID file.link AS nodo, estado
FROM "multiagente/subagents" WHERE tipo != "mapa" SORT orden
```

[[Multiagentes.canvas|Abrir el canvas de Multiagentes]] · [[Multiagentes]] · [[Runtime de agentes]] · [[Evals]]
