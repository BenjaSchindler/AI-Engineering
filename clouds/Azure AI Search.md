---
tipo: concepto
dominio: clouds
bloque: "03 · Recuperación RAG"
orden: 330
estado: por-ver
parent: "[[Clouds para Gen AI]]"
prereqs: ["[[RAG]]", "[[Modelos de embedding]]"]
se_evalua_con: ["[[Golden dataset]]"]
contrasta_con: ["[[Amazon Bedrock Knowledge Bases]]", "[[Vertex AI Vector Search]]"]
fuentes: ["https://learn.microsoft.com/en-us/azure/search/search-what-is-azure-search"]
aliases: ["Azure Cognitive Search"]
---
# Azure AI Search

## En el mapa

### Azure AI Search

![[assets/clouds/ai-search.svg|72]]

**Qué es:** Servicio de Azure para buscar y recuperar información empresarial en aplicaciones, agentes y sistemas RAG.

**Para qué sirve:** Permite consultar documentos con búsqueda de texto, vectores o ambas, además de filtros y capacidades de recuperación agéntica. Por ejemplo, un asistente busca políticas de garantía por país y producto, y entrega los fragmentos relevantes a un modelo de Microsoft Foundry para redactar la respuesta.

**Clave:** Su alcance supera un índice vectorial: consultar un índice clásico y usar recuperación agéntica implican flujos diferentes.

[[Amazon Bedrock Knowledge Bases]] · [[Vertex AI Vector Search]] · [[Clouds para Gen AI]]

## Qué gestiona realmente

Azure AI Search mantiene índices y ejecuta consultas sobre documentos con texto, vectores y campos de metadata. Puede formar la recuperación de un RAG; la aplicación consume evidencia y llama al modelo generador. El flujo clásico consulta un índice. La recuperación agéntica usa otra capa de planificación y recuperación, por lo que sus opciones no deben confundirse con una única consulta clásica. [Alcance del servicio](https://learn.microsoft.com/en-us/azure/search/search-what-is-azure-search).

## Qué se puede configurar

### Ingesta y esquema

Puedes subir documentos con vectores ya calculados o usar **indexers + skillsets** para extraer, dividir y vectorizar contenido. La vectorización integrada combina skills de fragmentación y embeddings durante la ingesta; un vectorizer compatible transforma preguntas en vectores al consultar. Automatiza trabajo, pero siguen siendo tuyas las decisiones de modelo, dimensiones, fragmentos y sincronización. [Vectorización integrada](https://learn.microsoft.com/en-us/azure/search/vector-search-integrated-vectorization).

Un esquema define campos: texto buscable, valores filtrables, campos retornables y vectores con dimensión y perfil. Los perfiles conectan algoritmo y compresión. Cambiar dimensiones o una representación incompatible requiere planificar la reconstrucción del índice.

| Configuración | Utilidad y tradeoff |
|---|---|
| HNSW / `exhaustiveKnn` | HNSW busca aproximadamente para reducir trabajo; exhaustive compara todos los vectores y sirve como referencia de recall. |
| `m`, `efConstruction`, `efSearch` | Controlan conectividad del grafo, esfuerzo de construcción y exploración en consulta. Más esfuerzo puede mejorar cobertura y elevar memoria, tiempo de indexación o latencia. |
| Métrica | Debe corresponder al embedding; por ejemplo, coseno para embeddings Azure OpenAI. No se elige comparando scores de distintos modelos como si fueran equivalentes. |
| Compresión | Cuantización escalar o binaria reduce representación; oversampling y rescoring pueden compensar pérdida de precisión con trabajo adicional. |

[Esquema, perfiles y parámetros](https://learn.microsoft.com/en-us/azure/search/vector-search-how-to-create-index).

## Recuperación: qué se ajusta por consulta

- **`k`:** candidatos de la consulta vectorial; **`top`:** resultados finales de la respuesta. En híbrida y reranking no son necesariamente el mismo conjunto ni tamaño.
- **Filtros:** restringen campos filtrables, por ejemplo país o producto. `vectorFilterMode` decide cómo aplicar el filtro respecto a búsqueda vectorial; el modo y la selectividad pueden cambiar cobertura y latencia.
- **Vectorización:** envías un vector preparado o texto para un vectorizer configurado. Documento y pregunta deben permanecer en el mismo espacio de embeddings.

[Consulta vectorial y filtros](https://learn.microsoft.com/en-us/azure/search/vector-search-how-to-query).

**Híbrida** ejecuta búsqueda textual y vectorial y combina rankings mediante RRF. Conserva la utilidad de palabras exactas como códigos y la de paráfrasis semánticas. La fusión reúne candidatos; no lee cada pasaje como un modelo de relevancia. [Búsqueda híbrida](https://learn.microsoft.com/en-us/azure/search/hybrid-search-overview).

**Semantic ranker** vuelve a evaluar los candidatos superiores usando sus campos de texto. Configuras campos prioritarios y activas ranking semántico en la consulta. Trabaja sobre hasta 50 candidatos: conviene suministrar suficientes candidatos para aprovecharlo. No genera nuevos documentos ni corrige una ingesta que perdió evidencia. Es diferente del rescoring de vectores comprimidos, que recalcula similitud. [Ranking semántico](https://learn.microsoft.com/en-us/azure/search/semantic-search-overview).

## Ejemplo conceptual y operación

Un índice de garantías contiene `contenido`, `contenidoVector`, `pais`, `producto`, `version` y `sourceUrl`. Un indexer divide los manuales y genera vectores. Ante «¿cubre el error E-104?», la aplicación filtra país y producto, consulta texto y vectores, combina resultados y usa semantic ranker antes de entregar los pasajes al LLM. El ejemplo necesita evaluar si las excepciones de garantía quedaron en el mismo fragmento que la regla.

Las **particiones** aportan capacidad de almacenamiento y distribución; las **réplicas**, capacidad de servicio de consultas y disponibilidad. Dimensiona con volumen, indexación y concurrencia reales: un cambio de capacidad no arregla un ranking pobre. [Planificación de capacidad](https://learn.microsoft.com/en-us/azure/search/search-capacity-planning).

Revisa errores de indexación, frescura, permisos y latencia de consulta junto con [[RAG evals]]. Mantén los filtros de acceso vinculados a la identidad y [[ACLs]]; el filtro de relevancia `pais=CL` no expresa por sí solo autorización.

## Diferencia frente a las otras opciones

[[Amazon Bedrock Knowledge Bases]] proporciona una capa RAG con modalidades de infraestructura gestionada y generación opcional. [[Vertex AI Vector Search]] clásico concentra su contrato en índices y vecinos. Azure AI Search resulta útil cuando necesitas un motor documental con búsqueda de texto y vectores, filtros, esquema y un pipeline de enriquecimiento opcional.

## Fuentes oficiales

- [Documentación oficial](https://learn.microsoft.com/en-us/azure/search/search-what-is-azure-search). Consulta: 2026-09-30.
