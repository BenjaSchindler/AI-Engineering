---
tipo: concepto
dominio: runtime
estado: por-ver
parent: "[[Runtime de agentes]]"
prereqs: ["[[Tools y function calling]]", "[[Contexto, memoria y estado]]"]
se_evalua_con: ["[[Tool evals]]", "[[Regresiones y CI]]"]
contrasta_con: []
fuentes: ["https://docs.langchain.com/oss/javascript/langgraph/persistence"]
bloque: "01 · Ejecución"
orden: 120
---
# Ejecución y recuperación

> **En una frase:** guardá el progreso y verificá qué ocurrió antes de repetir una acción.

```mermaid
flowchart LR
  a[Registrar intención] --> b[Ejecutar tool]
  b --> c[Guardar resultado]
  b -->|Timeout| d[Consultar estado de la acción]
  d --> e[Continuar o reintentar con protección]
```

## Tres ideas
- **Estado durable:** un checkpoint guarda el progreso fuera de la memoria del proceso. Persistir mensajes no demuestra que una acción externa terminó.
- **Idempotencia:** una clave estable por acción permite que un servicio compatible reconozca reintentos y evite duplicados.
- **Límites:** fijá pasos, tiempo y costo máximos; cuando se agotan, guardá el estado y terminá de forma explícita.

**Ejemplo:** crear una cotización da timeout. Consultá por su identificador antes de volver a crearla. Si no podés comprobarlo ni deduplicar, dejá la acción pendiente de revisión.

> [!TIP] Para recordar
> Timeout significa resultado desconocido; no prueba que la acción falló.

**Practicá:** ¿qué pasa si el proceso cae después de crear la cotización y antes de guardar su respuesta?

[[Streaming y cancelación]] · [[Trazas y debugging]]
