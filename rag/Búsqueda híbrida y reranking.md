---
tipo: concepto
dominio: rag
estado: por-ver
parent: "[[RAG]]"
prereqs: ["[[RAG básico]]", "[[Modelos de embedding]]"]
se_evalua_con: ["[[RAG evals]]"]
contrasta_con: []
fuentes:
  - https://www.elastic.co/docs/solutions/search/ranking
  - https://www.elastic.co/docs/reference/elasticsearch/rest-apis/reciprocal-rank-fusion
bloque: "03 · Índice y recuperación"
orden: 340
---
# Búsqueda híbrida y reranking

> **En una frase:** combiná palabras y significado para encontrar candidatos; después ordenalos por relevancia para la pregunta.

```mermaid
flowchart LR
  q[Consulta y permisos] --> b[BM25]
  q --> v[Vectores]
  b --> f[Fusionar rankings]
  v --> f
  f --> r[Reranker]
  r --> c[Contexto del LLM]
```

## Tres piezas
| Pieza | Para qué sirve |
|---|---|
| **BM25** | Puntúa coincidencias de términos; ayuda con nombres y códigos. No reemplaza un filtro exacto por ID |
| **Híbrida** | Une candidatos léxicos y vectoriales; cubre vocabulario exacto y paráfrasis |
| **Reranker** | Evalúa consulta y candidato juntos, por ejemplo con un cross-encoder; reordena lo recuperado |

**RRF** fusiona posiciones de rankings sin comparar directamente sus scores: suma `1 / (c + posición)` por lista. `c` suaviza el peso de las primeras posiciones; no es el número de resultados. [Referencia de RRF](https://www.elastic.co/docs/reference/elasticsearch/rest-apis/reciprocal-rank-fusion).

**Ejemplo:** “error E104 al iniciar sesión”. BM25 ayuda a encontrar `E104`; los vectores, explicaciones que dicen “fallo de acceso”. El reranker prioriza los pasajes que responden a esa combinación.

## Cómo decidir
- Compará vectorial → híbrida → híbrida con reranking usando las mismas preguntas.
- Medí recall de candidatos, nDCG del ranking final, calidad de respuesta, costo y latencia. Un reranker no rescata evidencia que nunca llegó.
- Aplicá [[ACLs]] en ambas búsquedas antes de exponer candidatos al reranker o al LLM. Ajustá cuántos recuperás y cuántos enviás al contexto.

Más etapas añaden costo: conservalas si tus evals muestran una mejora útil. [Ranking y reranking](https://www.elastic.co/docs/solutions/search/ranking).

**Practicá:** ¿qué revisarías si el pasaje correcto aparece entre 50 candidatos, pero no entre los 5 finales?

[[Caso práctico - Asistente de soporte]]
