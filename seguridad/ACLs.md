---
tipo: "concepto"
dominio: "seguridad"
parent: "[[Seguridad]]"
estado: por-ver
prereqs: ["[[Control humano y permisos]]"]
se_evalua_con: ["[[Golden dataset]]"]
contrasta_con: []
fuentes: []
bloque: "01 · Autorización"
orden: 120
---
# ACLs

> **En una frase:** cómo evito que un usuario vea datos que no debería. Los permisos viajan con cada chunk desde la fuente y se aplican **dentro** de la búsqueda, no después.

![Candado: los permisos filtran la búsqueda antes de llegar al LLM](../assets/acls.svg)

## Diagrama
```mermaid
flowchart LR
  u[Usuario] --> g["Resolver grupos<br/>user:ana · grupo:rrhh · org:todos"]
  g --> q["Query vectorial<br/>+ filtro acl ∈ grupos"]
  q --> db[("Vector DB<br/>cada chunk lleva acl[]")]
  db --> r[Solo chunks permitidos] --> llm[LLM]
```

## Pre-filtro vs post-filtro
```mermaid
flowchart LR
  subgraph mal["Post-filtro (mal)"]
    a[top-10 global] --> b[filtrar por acl] --> c["0 a 3 resultados<br/>o el LLM ya vio lo prohibido"]
  end
  subgraph bien["Pre-filtro (bien)"]
    d[top-10 entre los permitidos] --> e[Hasta 10 resultados permitidos]
  end
```
Post-filtrar el top-k puede dejar la lista vacía, y si el filtro lo hace el prompt en vez de la DB, el dato ya se filtró al contexto.

## Cómo modelarlo
- Cada chunk guarda `acl: ["user:…", "grupo:…"]` heredado del documento ([[Normalization]] lo pone en el esquema).
- El [[Connector]] trae los permisos de la fuente (Drive sharing, Notion members) y sus cambios.
- Cambio de permisos = evento de [[Incremental sync]]: hay que re-escribir la metadata aunque el texto no cambie.

## Trade-offs
- Filtros muy selectivos degradan [[ANN HNSW]]: el grafo pierde vecinos válidos. Probá con tus grupos reales.
- [[Caché de respuestas]] por encima del filtro filtra datos: la key debe incluir la identidad.
- Para auditoría, loggeá qué chunks se mostraron a quién.

## También en multimedia
En [[RAG multimedia]], el candado acompaña el texto extraído, las imágenes, los frames, las miniaturas y el archivo original. Comprobá permisos al recuperar el original e invalidá caches cuando cambien; la identidad sola no refleja una revocación.

[[Seguridad]] reúne este control de lectura con [[Guardrails]] y [[Control humano y permisos]] para las acciones.
