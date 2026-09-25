---
tipo: concepto
dominio: runtime
estado: por-ver
parent: "[[Runtime de agentes]]"
prereqs: ["[[Ejecución y recuperación]]", "[[Resiliencia entre proveedores]]"]
se_evalua_con: ["[[Regresiones y CI]]", "[[Evals por cliente y producción]]"]
contrasta_con: []
fuentes:
  - https://learn.microsoft.com/en-us/azure/architecture/patterns/queue-based-load-leveling
  - https://sre.google/workbook/canarying-releases/
bloque: "03 · Operación"
orden: 330
---
# Despliegue y operación bajo carga

> **En una frase:** limitá cuánto trabajo aceptás y publicá cambios de a poco, con criterios para detenerlos.

```mermaid
flowchart LR
  p[Peticiones] --> a{¿Hay capacidad?}
  a -->|Sí| q[Cola acotada]
  a -->|No| x[Informar límite y reintento]
  q --> w[Workers con concurrencia limitada]
  w --> m[Modelo y tools]
```

## Absorber picos
- **Cola:** desacopla llegada y procesamiento; sirve para tareas que toleran espera. Medí profundidad y edad del trabajo pendiente.
- **Concurrencia y cuotas:** limitá llamadas simultáneas y consumo por cliente/proveedor. Cada tarea conserva timeout y presupuesto.
- **Backpressure:** frená o rechazá entradas cuando no hay capacidad; una cola infinita convierte sobrecarga en espera infinita. Los reintentos de mensajes requieren [[Ejecución y recuperación|idempotencia]].

Una cola amortigua picos, pero no aumenta la capacidad sostenida del sistema. [Patrón de colas](https://learn.microsoft.com/en-us/azure/architecture/patterns/queue-based-load-leveling).

## Publicar con control
1. **Objetivos:** definí SLOs —objetivos de servicio— con ventana y población: por ejemplo, 99% de tareas sin error técnico en 7 días. Medí calidad y costo aparte.
2. **Canary:** tras pasar evals, enviá una fracción pequeña de tráfico a la versión nueva y comparala con la anterior.
3. **Avanzar o revertir:** fijá antes límites de errores, latencia, calidad y costo; reuní evidencia suficiente antes de ampliar tráfico.

Versioná juntos prompt, modelo, tools e índice. Volver atrás requiere compatibilidad del estado; no deshace acciones externas. [Despliegues canary](https://sre.google/workbook/canarying-releases/).

**Practicá:** si llegan tareas más rápido de lo que salen, ¿qué límite protege al usuario y al proveedor?

[[Operación en producción]] · [[Trazas y debugging]]
