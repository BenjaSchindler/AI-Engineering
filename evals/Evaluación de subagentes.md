---
tipo: eval
dominio: evals
estado: por-ver
parent: "[[Evals]]"
prereqs: ["[[Subagents]]", "[[Graders]]"]
se_evalua_con: []
contrasta_con: []
fuentes:
  - https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents
  - https://www.anthropic.com/engineering/multi-agent-research-system
bloque: "03 · Coordinación"
orden: 320
---
# Evaluación de subagentes

> **En una frase:** evaluá cada subagente, la coordinación y el resultado completo; compará también contra un solo agente.

```mermaid
flowchart LR
  a[Caso de prueba] --> b[Subagente]
  b --> c[Coordinación e integración]
  c --> d[Resultado final y costo total]
```

## Cuatro preguntas
| Nivel | Qué comprobar |
|---|---|
| Decisión | Si correspondía delegar, a quién y con qué contexto |
| Subagente | Cumple su tarea con el contexto recibido y aporta evidencia |
| Coordinación | Delegación útil, sin duplicados ni pérdida de información al integrar |
| Sistema | Éxito final, permisos, costo total y latencia |

**Ejemplo:** el subagente detecta una exclusión, pero el principal la omite. La extracción funciona; la integración falla.

> [!TIP] Para recordar
> Probá timeouts, cancelación y resultados contradictorios. Repetí casos y aceptá caminos distintos si cumplen las restricciones.

## Practicá
> [!question]- ¿Cómo demostrarías que tus subagentes aportan algo frente a un solo agente?
> Corré los mismos casos con un solo agente con presupuesto comparable, en tokens, costo y tiempo, y con el sistema de subagentes; compará éxito final, costo total y latencia. Después retirá un subagente a la vez y medí qué hallazgos o checks dejan de cumplirse. Si al retirarlo no cae nada, no aporta; si cae algo, sabés qué aporte justifica su costo. Sin el presupuesto comparable, el sistema con subagentes puede ganar solo porque gastó más.

## Cómo probarlo
[[Decisión de delegar]] → [[Métricas de subagentes]] → [[Casos de eval de subagentes]] → [[Evals de Deep Agents]].

## Se conecta con
[[Multi-agent evals]] · [[Golden dataset]] · [[Regresiones y CI]] · [[Trazas y debugging]]
