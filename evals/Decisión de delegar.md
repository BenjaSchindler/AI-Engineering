---
tipo: eval
dominio: evals
estado: por-ver
parent: "[[Evals]]"
prereqs: ["[[Evaluación de subagentes]]"]
se_evalua_con: []
contrasta_con: []
fuentes:
  - https://docs.langchain.com/langsmith/evaluate-complex-agent
bloque: "03 · Coordinación"
orden: 330
---
# Decisión de delegar

> **En una frase:** definí cuándo delegar es obligatorio, opcional o no corresponde, antes de puntuar la llamada.

```mermaid
flowchart LR
  a[Estado previo a decidir] --> b[Principal elige una acción]
  b --> c[Comparar con decisiones aceptables]
```

## Tres casos
Ejemplos de una política de prueba, no reglas universales:

| Etiqueta | Caso | Qué aceptar |
|---|---|---|
| Obligatorio | Solo el especialista autorizado puede verificar cobertura | Invocarlo con los datos necesarios |
| No corresponde | Falta identificar la póliza | Pedir aclaración antes de delegar |
| Opcional | Ambos agentes pueden resolver la consulta | Cualquiera de los caminos si cumple calidad y presupuesto |

Evaluá con la información disponible **en ese momento**, no con hallazgos posteriores. Revisá destino, argumentos y permisos.

**Métricas:** omisiones / casos obligatorios; delegaciones indebidas / casos donde no corresponde. Si no hay casos de una categoría, reportá “no aplica”.

> [!TIP] Para recordar
> Una llamada opcional no es innecesaria por definición. Compará calidad, costo y latencia con y sin delegación.

**Practicá:** ¿permitirías dos especialistas distintos si ambos pueden resolver correctamente la tarea?

[[Métricas de subagentes]] · [[Casos de eval de subagentes]] · [[Tool evals]] · [[Regresiones y CI]]
