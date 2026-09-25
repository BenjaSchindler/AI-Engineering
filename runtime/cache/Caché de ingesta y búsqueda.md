---
tipo: "concepto"
dominio: "runtime"
parent: "[[Caché en agentes]]"
estado: por-ver
prereqs: ["[[Claves e invalidación de caché]]", "[[Document IDs]]"]
se_evalua_con: ["[[Evals de caché]]"]
contrasta_con: []
fuentes: []
aliases: ["Caching"]
bloque: "02 · Capas de reutilización"
orden: 230
---
# Caché de ingesta y búsqueda

> **En una frase:** reutilizá resultados de ingesta y búsqueda mientras coincidan contenido, configuración y permisos.

![Rayo: reutilizar resultados con una clave que identifica su configuración](../../assets/caching.svg)

## Diagrama
```mermaid
flowchart LR
  req[Input] --> k["Clave: contenido, configuración y alcance"]
  k --> c{¿Entrada vigente y autorizada?}
  c -->|hit| out[Devolver]
  c -->|miss| compute["Calcular<br/>embedding / parseo / retrieval"]
  compute --> store[(Guardar con versión y expiración)] --> out
```

## Qué cachear
| Capa | Key | TTL | Ahorra |
|---|---|---|---|
| Embeddings | `hash(input, modelo, versión, dimensiones, modo, preprocesamiento)` | Mientras coincidan el contenido y toda la configuración | Casi todo el costo de re-ingesta |
| Retrieval | Query, filtros, versión del índice, cliente y alcance de acceso | Según frescura requerida; invalidar cambios | Búsquedas repetidas |
| Parseo / OCR | Hash del archivo, versión del parser y opciones; alcance | Hasta cambiar alguna dependencia | Relectura del mismo documento |

## Reglas
- Cambiar modelo, contenido o preprocesamiento requiere otra entrada de embeddings.
- Con [[Incremental sync]] + cache de embeddings, re-sincronizar cuesta solo lo que cambió.
- Aplicá [[ACLs]] también en hits; un permiso revocado no espera al TTL.

> [!TIP] Otras capas
> [[Caché en agentes]] compara [[Prompt caching]] y [[Caché de respuestas]]. Elegí según costo medido, repetición y riesgo de desactualización.
