---
tipo: concepto
dominio: clouds
bloque: "03 · Recuperación RAG"
orden: 320
estado: por-ver
parent: "[[Clouds para Gen AI]]"
prereqs: ["[[RAG]]", "[[Modelos de embedding]]"]
se_evalua_con: ["[[Golden dataset]]"]
contrasta_con: ["[[Amazon Bedrock Knowledge Bases]]", "[[Azure AI Search]]"]
fuentes: ["https://docs.cloud.google.com/gemini-enterprise-agent-platform/build/vector-search/overview", "https://docs.cloud.google.com/gemini-enterprise-agent-platform/build/vector-search-2/overview"]
aliases: ["Agent Platform Vector Search"]
---
# Vertex AI Vector Search

## En el mapa

### Vertex AI Vector Search

![[assets/clouds/vector-search.svg|72]]

**Qué es:** Servicio de Google Cloud que busca elementos similares en índices de representaciones numéricas, llamados vectores.

**Para qué sirve:** Recupera candidatos mediante búsqueda vectorial, dispersa o híbrida, con filtros. Por ejemplo, una aplicación divide manuales en fragmentos, genera embeddings y busca los pasajes más relacionados con la pregunta del cliente para entregarlos a un modelo en un flujo RAG.

**Clave:** El índice clásico sigue siendo Vector Search; Agent Retrieval, antes Vector Search 2.0, es un producto distinto dentro de Gemini Enterprise Agent Platform.

[[Amazon Bedrock Knowledge Bases]] · [[Azure AI Search]] · [[Clouds para Gen AI]]

## Qué gestiona realmente

El producto clásico gestiona índices para búsqueda de vecinos, su despliegue y consultas. Tu aplicación prepara fragmentos y embeddings, conserva el contenido asociado a sus identificadores y decide cómo construir la respuesta. Puede servir tanto RAG como recomendaciones o búsqueda de imágenes. [Panorama de Vector Search](https://docs.cloud.google.com/gemini-enterprise-agent-platform/build/vector-search/overview).

**Agent Retrieval, antes Vector Search 2.0, es distinto:** usa Collections de Data Objects, almacena payload y vectores juntos, puede generar embeddings y reduce la administración de infraestructura. No hay que trasladar automáticamente sus capacidades al índice clásico. La documentación está ahora bajo **Gemini Enterprise Agent Platform**, aunque nombres de APIs y rutas históricas puedan seguir apareciendo. [Diferencia oficial entre ambos productos](https://docs.cloud.google.com/gemini-enterprise-agent-platform/build/vector-search-2/overview).

## Qué se puede configurar

| Capa | Opciones y efecto |
|---|---|
| Preparación externa | Tú eliges parser, tamaño de fragmento, solapamiento y modelo de embeddings. El índice clásico recibe representaciones preparadas; no define por sí mismo la unidad documental adecuada. |
| Dimensión y métrica | `dimensions` debe coincidir con el vector denso. `distanceMeasureType` y normalización determinan cómo comparar. Se admiten L2, L1, producto punto y coseno; para sparse se usa producto punto. La métrica debe concordar con el modelo. |
| Algoritmo | Tree-AH ofrece búsqueda aproximada; brute force compara todos los vectores. La búsqueda exacta sirve como referencia para medir recall, con más trabajo por consulta. |
| Precisión aproximada | `approximateNeighborsCount` controla candidatos antes de reordenar por distancia exacta; `fractionLeafNodesToSearch` fija la fracción de hojas exploradas por defecto en el índice. La documentación actual declara `leafNodesToSearchPercent` obsoleto, aunque aún aparece en ejemplos y SDK. Más exploración suele mejorar recall con más latencia. |
| Partición | El tamaño de shard distribuye el índice para servirlo. Se elige según volumen y operación, no como sustituto de evaluar relevancia. |

[Parámetros del índice](https://docs.cloud.google.com/gemini-enterprise-agent-platform/build/vector-search/configuring-indexes).

### Consulta, filtros y búsqueda híbrida

En cada consulta puedes sobrescribir la exploración con `fraction_leaf_nodes_to_search_override` del SDK Python (`fractionLeafNodesToSearchOverride` en el mensaje API), y los candidatos aproximados con `approx_num_neighbors`. Estos ajustes de consulta son distintos de los valores del índice y de `num_neighbors`, que pide resultados finales. [Referencia del SDK](https://docs.cloud.google.com/python/docs/reference/aiplatform/latest/google.cloud.aiplatform.MatchingEngineIndexEndpoint).

Los **restricts** permiten limitar vecinos por atributos de texto o valores numéricos: país, categoría o precio. Debes cargar estos atributos y formular los filtros al consultar. Un filtro reduce el universo elegible; no mejora la representación semántica de los documentos. [Filtrado](https://docs.cloud.google.com/gemini-enterprise-agent-platform/build/vector-search/filtering).

La búsqueda **híbrida** combina representaciones densas y sparse. Las sparse capturan términos; las densas, similitud semántica. Debes producir ambas representaciones y ajustar su combinación, por ejemplo mediante el peso de fusión RRF. No es simplemente cargar texto y habilitar BM25 automáticamente. [Funcionamiento y combinación híbrida](https://docs.cloud.google.com/gemini-enterprise-agent-platform/build/vector-search/about-hybrid-search).

El número de vecinos pedidos (`num_neighbors` en el SDK) controla los candidatos devueltos. [Consulta del endpoint](https://docs.cloud.google.com/gemini-enterprise-agent-platform/build/vector-search/query-index-public-endpoint). Es una decisión distinta de cuánto explorar durante la búsqueda aproximada: pedir más resultados no garantiza recuperar un vecino que el algoritmo no visitó. Después puedes aplicar un reranker semántico externo; el reordenamiento exacto por distancia del índice no equivale a un cross-encoder que lee pregunta y pasaje. [Recuperación y ranking externo](https://docs.cloud.google.com/gemini-enterprise-agent-platform/build/vector-search/overview).

## Ejemplo conceptual y operación

Para manuales de garantías, tu pipeline divide texto, genera embeddings y guarda `id → fragmento y fuente`. Carga vectores con `pais=CL` y `producto=X`. La consulta vectoriza la pregunta con el mismo modelo, filtra candidatos y obtiene identificadores; luego busca el texto, aplica reranking si hace falta y construye el contexto del LLM.

Al crear el índice eliges `BATCH_UPDATE` o `STREAM_UPDATE`. Batch procesa actualizaciones desde archivos de Cloud Storage; streaming permite upserts y borrados de datapoints con propagación casi en tiempo real. La elección refleja si publicas versiones periódicas del corpus o necesitas cambios continuos. [Creación y método de actualización](https://docs.cloud.google.com/gemini-enterprise-agent-platform/build/vector-search/create-manage-index) · [Semántica de los métodos](https://docs.cloud.google.com/vertex-ai/docs/reference/rpc/google.cloud.aiplatform.v1#indexupdatemethod).

Un índice se despliega en un endpoint. Puedes configurar recursos de servicio y réplicas para atender carga; el diseño de red pública o privada define cómo llega la aplicación. Conserva trazabilidad entre versión del embedding, índice y documento. [Despliegue y administración](https://docs.cloud.google.com/gemini-enterprise-agent-platform/build/vector-search/deploy-index-public).

Evalúa dos preguntas diferentes: ¿el índice aproximado encuentra los vecinos que encontraría búsqueda exacta?, y ¿esos vecinos contienen evidencia útil para la respuesta? La primera mide recall del ANN; la segunda pertenece a [[RAG evals]]. Un índice puede cumplir la primera y fallar la segunda por mal chunking o embeddings.

## Diferencia frente a las otras opciones

[[Amazon Bedrock Knowledge Bases]] incorpora una capa de pipeline RAG y generación opcional. [[Azure AI Search]] aporta búsqueda textual, esquema documental y enriquecimiento integrado. Vector Search clásico resulta útil cuando quieres controlar la preparación y composición del pipeline, mientras Google gestiona la infraestructura de búsqueda de vecinos.

## Fuentes oficiales

- [Documentación oficial](https://docs.cloud.google.com/gemini-enterprise-agent-platform/build/vector-search/overview). Consulta: 2026-09-30.
- [Agent Retrieval: anteriormente Vector Search 2.0](https://docs.cloud.google.com/gemini-enterprise-agent-platform/build/vector-search-2/overview).
