---
tipo: pipeline
dominio: rag
parent: "[[Vector DB]]"
estado: por-ver
prereqs: ["[[Connector]]"]
se_evalua_con: []
contrasta_con: []
fuentes: []
---
# Document IDs

> **En una frase:** cómo sigo el mismo documento a lo largo del tiempo. El ID viene de la fuente, no del contenido, así un doc editado o movido sigue siendo el mismo doc.

## Diagrama
```mermaid
flowchart LR
  src["source_id<br/>(fileId, pageId, URL canónica)"] --> id["doc_id = source:source_id"]
  id --> c0["chunk_id = doc_id#0"]
  id --> c1["chunk_id = doc_id#1"]
  id --> cn["chunk_id = doc_id#n"]
  id -.->|el contenido cambia| v["content_hash cambia<br/>doc_id se mantiene"]
```

## Qué ID usar
| Fuente | ID estable | Trampa |
|---|---|---|
| Drive / Notion | fileId / pageId | El título cambia, el ID no |
| Web | URL canónica (sin utm, sin trailing slash) | Redirects y duplicados |
| DB | tabla + primary key | Migraciones que renumeran |
| Archivos locales | ruta, o hash si son inmutables | Mover = "nuevo doc" si usás la ruta |

## Para qué sirve tener IDs estables
- [[Incremental sync]]: "modificado" solo existe si podés decir "es el mismo doc".
- Upsert en la [[Vector DB]]: borrás todos los `chunk_id` con prefijo `doc_id` y reescribís.
- Citas: el usuario ve la fuente, no un chunk anónimo.
- [[Deduplication]] separa "mismo doc actualizado" de "otro doc copiado".

> [!TIP] Determinístico > aleatorio
> Nunca uses UUID random en ingesta: reprocesás el mismo doc, obtenés otro ID, y ahora tenés dos.
