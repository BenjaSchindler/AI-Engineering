---
tipo: pipeline
dominio: rag
parent: "[[RAG]]"
estado: por-ver
prereqs: []
se_evalua_con: []
contrasta_con: []
fuentes: []
bloque: "02 · Ingesta"
orden: 210
---
# Connector

> **En una frase:** cómo obtengo los datos. El connector habla el idioma de cada fuente (API, DB, archivos) y entrega documentos crudos al pipeline de [[Vector DB]].

![Enchufe: conectar la fuente y traer contenido con permisos](../../assets/connector.svg)

## Diagrama
```mermaid
flowchart LR
  A[Google Drive] --> C
  B[Notion / Confluence] --> C
  D[(Postgres)] --> C
  E[S3 / archivos] --> C
  W[Web / crawler] --> C
  C{{"Connector<br/>auth · paginación · rate limit · reintentos"}} --> R[Docs crudos + metadata de origen]
```

## Tres formas de traer datos
| Modo | Cómo | Cuándo |
|---|---|---|
| Pull (polling) | Preguntás cada N minutos qué hay | Simple, sirve para casi todo |
| Webhook / push | La fuente te avisa cuando algo cambia | La frescura importa y la fuente lo soporta |
| Change feed / CDC | Leés el log de cambios de la DB | Volumen alto, necesitás los borrados |

## Lo que cada connector tiene que devolver
- Contenido crudo sin transformar → transformar es trabajo de [[Normalization]].
- `source_id` estable de la fuente → base de los [[Document IDs]].
- `updated_at` o cursor → base del [[Incremental sync]].
- Quién puede verlo → base de los [[ACLs]].

> [!WARNING] Errores clásicos
> Olvidar los borrados (la fuente ya no tiene el doc pero vos sí) y no respetar rate limits (te bloquean la API).
