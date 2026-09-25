---
tipo: eval
dominio: evals
parent: "[[Evals]]"
estado: por-ver
prereqs: []
se_evalua_con: []
contrasta_con: []
fuentes: []
bloque: "01 · Casos y criterios"
orden: 110
---
# Golden dataset

> **En una frase:** un conjunto curado y versionado de casos con criterios de éxito. Puede incluir respuestas, acciones y estados esperados.

Un [[Golden cases agénticos|golden case]] puede tener documentos, varios mensajes y checks intermedios. El dataset reúne esos casos.

## Diagrama
```mermaid
flowchart LR
  a["Logs reales<br/>preguntas de usuarios"] --> cur
  b["Expertos<br/>casos que importan"] --> cur
  c["Sintéticos<br/>LLM genera variaciones"] --> cur
  cur[Curar: dedup, balancear, etiquetar] --> ds[("golden v1.2<br/>en git")]
  ds --> split[Split: dev / holdout]
```

## Un caso
```json
{
  "id": "faq-042",
  "input": "¿Cuántos días de vacaciones tengo después de 5 años?",
  "expected": "20 días hábiles",
  "expected_sources": ["drive:1a2b3c#3"],
  "tags": ["rrhh", "multi-hop:no", "dificultad:media"],
  "user_context": { "grupos": ["grupo:empleados"] }
}
```

## Reglas
- Empezá con casos bien elegidos; el tamaño depende de tareas, clientes y riesgos. Cada caso tiene que "atrapar" algo.
- Cubrí los bordes: preguntas sin respuesta en los datos, ambiguas, con [[ACLs]] restrictivos, en otro idioma.
- Versionalo en git. Cambiar dataset y prompt a la vez = no sabés qué mejoró.
- Holdout que nadie mira: si optimizás contra el dev set, el número deja de significar algo.
- Cada fallo reportado por un usuario → un caso nuevo.

## Se conecta con
Alimenta [[Evals]], [[Tool evals]] y [[Multi-agent evals]]. Mide el recall de [[ANN HNSW]] y la calidad de [[RAG básico]].

Para comparar [[Chunking]] y [[Modelos de embedding]], etiquetá documento + pasaje fuente estable además del ID del chunk: el ID puede cambiar al volver a cortar. En [[RAG multimedia]], la evidencia esperada incluye página, región o intervalo de tiempo.

[[RAG evals]] reúne Recall@k, precisión, ranking y calidad de respuesta para puntuar estos casos.

Para puntuar y comparar: [[Graders]] → [[Regresiones y CI]]. Segmentá con [[Evals por cliente y producción]].
