---
tipo: concepto
dominio: rag
cssclasses: [rag-visual]
parent: "[[RAG]]"
estado: por-ver
prereqs: ["[[Modelos de embedding]]", "[[Vector DB]]"]
se_evalua_con: ["[[RAG evals]]"]
contrasta_con: ["[[Cross-encoder]]"]
aliases: [Biencoder, Bi encoder]
fuentes:
  - https://www.sbert.net/examples/cross_encoder/applications/README.html
bloque: "03 · Índice y recuperación"
orden: 350
---
# Bi-encoder

> **En una frase:** convierte consulta y pasajes por separado en vectores que se pueden comparar.

![RAG visual: Bi-encoder con dos entradas independientes](../assets/bi-encoder.svg)

- **Ingesta:** los pasajes se codifican una vez y sus vectores se guardan en [[Vector DB]].
- **Consulta:** después de [[Query rewrite]], codificás la pregunta y buscás vectores cercanos con encoders compatibles.
- **Salida:** candidatos por similitud; se pueden fusionar con BM25 en [[Búsqueda híbrida y reranking]].

Permite buscar rápido en muchos pasajes. El [[Cross-encoder]] puede afinar el orden después. [Referencia: bi-encoder y cross-encoder](https://www.sbert.net/examples/cross_encoder/applications/README.html).
