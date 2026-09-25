---
tipo: eval
dominio: evals
estado: por-ver
parent: "[[Evals]]"
prereqs: ["[[Golden cases agénticos]]", "[[Graders]]"]
se_evalua_con: []
contrasta_con: []
fuentes:
  - https://docs.langchain.com/langsmith/trajectory-evals
  - https://docs.langchain.com/langsmith/evaluate-complex-agent
bloque: "02 · Qué evaluar"
orden: 240
---
# Evals de trayectoria

> **En una frase:** evaluá decisiones observables durante la ejecución, además del resultado final.

```mermaid
flowchart LR
  a[Estado disponible] --> b[Decisión y tool]
  b --> c[Resultado de la tool]
  c --> d[Siguiente decisión]
```

## Tres clases de checks
| Check | Ejemplo | Cómo |
|---|---|---|
| Acción y argumentos | Guarda para el cliente correcto | Código sobre llamada y resultado |
| Orden parcial | Lee el endoso antes de persistir el valor actualizado | Código sobre eventos |
| Uso de información | Aplica una excepción sin perder su condición | Regla explícita o juez con evidencia |

**Métricas:** checks aprobados / aplicables; casos sin violaciones críticas / casos. Mostrá ambos: muchos aciertos no compensan una acción prohibida.

## Dos formas de ejecutar
- **Paso aislado:** reconstruí un estado válido y evaluá la próxima decisión. Ayuda a localizar el error.
- **End-to-end:** dejá que el agente produzca su recorrido real. Detecta errores acumulados.

> [!TIP] Para recordar
> En AgentEvals, `strict` exige orden, `unordered` permite reordenar, `superset` admite llamadas extra y `subset` restringe a las de referencia. Ninguno verifica por sí solo todo el resultado de negocio; agregá tus checks.

Evaluá con lo conocido en ese paso, sin exigir un único recorrido ni acceder a razonamiento interno. Que una tool haya sido llamada no prueba que tuvo éxito.

## Practicá
> [!question]- Tu referencia espera un grep, pero el agente leyó el archivo directo y respondió bien. ¿Falla el caso?
> Depende de si el grep era obligatorio por una razón que puedas nombrar. Si la política exige buscar, por ejemplo porque leer el archivo entero rompe el presupuesto o porque el grep deja evidencia auditable, ese check falla. Si no, la lectura directa es un camino válido y la referencia estaba sobreespecificada: aceptá los dos caminos o evaluá el resultado en lugar del recorrido exacto. Puntuá lo que importaba, usar el dato correcto dentro del presupuesto, no el recorrido que imaginaste.

## Se conecta con
[[Tool evals]] · [[Métricas de subagentes]]
