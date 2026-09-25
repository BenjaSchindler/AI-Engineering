---
tipo: concepto
dominio: runtime
estado: por-ver
parent: "[[Runtime de agentes]]"
prereqs: ["[[Ejecución y recuperación]]"]
se_evalua_con: ["[[Regresiones y CI]]"]
contrasta_con: []
fuentes: ["https://platform.claude.com/docs/en/api/errors"]
bloque: "01 · Ejecución"
orden: 140
---
# Resiliencia entre proveedores

> **En una frase:** clasificá la falla antes de decidir si corregir, esperar o cambiar de proveedor.

```mermaid
flowchart LR
  a[Falla] --> b{Tipo}
  b -->|Petición inválida| c[Corregir petición]
  b -->|Transitoria| d[Reintento limitado]
  d -->|Persiste| e[Fallback compatible o detener]
```

## Tres ideas
- **Reintentos:** para fallas transitorias, usá espera creciente y variación aleatoria —backoff y jitter—; respetá la espera indicada por el proveedor.
- **Protección:** limitá intentos, concurrencia, tiempo total y gasto. Un circuit breaker pausa llamadas a un proveedor que falla repetidamente.
- **Fallback:** verificá compatibilidad de tools, mensajes, contexto y permisos de datos. Medí también si cambia la calidad.

**Ejemplo:** un esquema inválido necesita corrección; repetirlo diez veces solo consume recursos. Un timeout exige además revisar posibles efectos ya ejecutados.

> [!TIP] Para recordar
> Los reintentos del SDK y del runtime pueden multiplicarse. Definí quién controla el presupuesto total.

## Practicá
> [!question]- ¿Qué conservarías al cambiar de proveedor a mitad de una ejecución?
> El estado de la tarea: IDs, decisiones y restricciones, resultados de tools ya ejecutadas con sus claves de idempotencia, qué acciones están hechas o pendientes, y el presupuesto consumido. Adaptá mensajes y definiciones de tools al formato del nuevo proveedor y volvé a contar tokens: otro tokenizer u otra ventana puede hacer que el mismo historial no entre ([[Presupuesto de contexto]]). No arrastres lo que solo entiende el proveedor anterior, como su caché o sus bloques de razonamiento. Y comprobá que el nuevo proveedor esté autorizado para esos datos.

## Se conecta con
[[Presupuesto de contexto]] · [[Evals por cliente y producción]]
