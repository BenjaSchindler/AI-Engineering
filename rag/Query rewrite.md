---
tipo: técnica
dominio: rag
cssclasses: [rag-visual]
parent: "[[RAG]]"
estado: por-ver
prereqs: ["[[RAG básico]]"]
se_evalua_con: ["[[RAG evals]]"]
contrasta_con: []
aliases: [Query rewriting]
fuentes:
  - https://learn.microsoft.com/en-us/azure/search/semantic-how-to-query-rewrite
bloque: "03 · Índice y recuperación"
orden: 340
---
# Query rewrite

> **En una frase:** reformulá la pregunta para que el buscador entienda qué necesitás, conservando su intención.

![RAG visual: Query rewrite antes de buscar](../assets/query-rewrite.svg)

- **Entrada:** pregunta + contexto relevante del chat. “¿Y cómo lo renuevo?” → “¿Cómo renuevo mi pasaporte?” si ese era el tema.
- **Lugar:** antes del embedding y de BM25; la consulta clara pasa a [[Bi-encoder]] y [[Búsqueda híbrida y reranking|búsqueda híbrida]].
- **Cuidado:** conservá códigos, fechas y restricciones. Si falta contexto, pedí aclaración; no inventes el referente.

Es opcional y agrega latencia. Compará el retrieval con la pregunta original y la reformulada. [Referencia: query rewriting](https://learn.microsoft.com/en-us/azure/search/semantic-how-to-query-rewrite).
