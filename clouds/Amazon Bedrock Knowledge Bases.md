---
tipo: concepto
dominio: clouds
bloque: "03 · Recuperación RAG"
orden: 310
estado: por-ver
parent: "[[Clouds para Gen AI]]"
prereqs: ["[[RAG]]", "[[Modelos de embedding]]"]
se_evalua_con: ["[[Golden dataset]]"]
contrasta_con: ["[[Vertex AI Vector Search]]", "[[Azure AI Search]]"]
fuentes: ["https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base.html"]
aliases: []
---
# Amazon Bedrock Knowledge Bases

## En el mapa

### Amazon Bedrock Knowledge Bases

![[assets/clouds/bedrock-knowledge-bases.svg|72]]

**Qué es:** Servicio de AWS que conecta fuentes propias con recuperación y generación para construir aplicaciones RAG.

**Para qué sirve:** Busca información relevante y la entrega al modelo como contexto para responder con evidencia. Por ejemplo, un asistente recupera pasajes de políticas de devolución y manuales de soporte antes de explicar qué garantía corresponde a una compra.

**Clave:** Cubre un flujo RAG gestionado, con distintas modalidades; su alcance es mayor que el de un índice vectorial y requiere controlar la calidad y vigencia de las fuentes.

[[Vertex AI Vector Search]] · [[Azure AI Search]] · [[Clouds para Gen AI]]

## Qué gestiona realmente

Knowledge Bases conecta ingesta, recuperación y generación. En el flujo Customer-managed, `Retrieve` entrega evidencia a tu aplicación y `RetrieveAndGenerate` agrega una respuesta con citas. [Recuperación y generación](https://docs.aws.amazon.com/bedrock/latest/userguide/kb-test-retrieve-generate.html). La documentación actual distingue dos modalidades: **Managed Knowledge Base**, que gestiona ingesta, índice, almacenamiento y recuperación, y **Customer-managed Knowledge Base**, donde eliges y operas el almacén y sus configuraciones. En Managed, embeddings y reranking tienen modelos gestionados por defecto, con opciones de personalización; también existen recuperación agéntica y filtrado por permisos documentales en conectores compatibles. [Alcance y modalidades](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base.html).

Las opciones siguientes describen principalmente el flujo con **vector store elegido por el cliente**. No conviene atribuir cada parámetro a todas las modalidades. Managed también utiliza `Retrieve`, pero con `managedSearchConfiguration` en vez de `vectorSearchConfiguration`; su recuperación siempre es híbrida y permite controlar el reranking gestionado, personalizado o desactivado. [Consulta de Managed](https://docs.aws.amazon.com/bedrock/latest/userguide/kb-test-retrieve.html).

## Qué se puede configurar

| Decisión | Configuración y efecto |
|---|---|
| Fuentes y permisos | Seleccionas la fuente y un rol IAM para acceder a los servicios. Una sincronización prepara el contenido para recuperar; la aplicación necesita comprobar que la evidencia corresponde a la versión vigente. |
| Embeddings y almacenamiento | Eliges modelo, dimensiones y tipo de vector donde sean compatibles, además del almacén y mapeo de campos. Cambiar el modelo exige volver a generar representaciones coherentes para documentos y preguntas. |
| Índice y métrica | La configuración del índice depende del backend elegido. Knowledge Bases no expone un único conjunto universal de parámetros ANN o de métricas para todos los almacenes. |

[Creación, modelos y vector stores](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base-create.html).

### Parsing y fragmentos

El parsing extrae el contenido; el chunking decide qué unidad se recuperará. Puedes usar fragmentos de tamaño fijo con solapamiento, fragmentación jerárquica, semántica o sin fragmentación. Tamaño pequeño favorece precisión; tamaño grande conserva contexto pero puede añadir contenido irrelevante. El solapamiento evita cortar una idea a costa de duplicación.

En **jerárquica** se recuperan hijos y se sustituyen por padres más amplios: varios hijos del mismo padre pueden producir menos resultados finales. En **semántica**, máximo de tokens, buffer de oraciones y umbral de separación controlan dónde se corta; incorpora procesamiento adicional. [Estrategias de chunking](https://docs.aws.amazon.com/bedrock/latest/userguide/kb-chunking.html).

### Recuperación en cada pregunta

- **`numberOfResults`:** limita los resultados candidatos. Más candidatos pueden mejorar cobertura, pero aumentan el contexto y trabajo posterior; en jerárquica cuenta hijos antes de consolidarlos.
- **Filtros de metadata:** restringen país, producto, fecha o categoría. Deben estar presentes en la ingesta y sus operadores dependen de la modalidad y almacenamiento.
- **`SEMANTIC` / `HYBRID`:** selecciona embeddings solos o texto más embeddings. Híbrida necesita un backend compatible y campo de texto filtrable; la guía actual enumera RDS, OpenSearch Serverless y MongoDB. Es útil para códigos exactos junto a preguntas con paráfrasis.
- **Reranking:** vuelve a ordenar candidatos con un modelo de relevancia. Puede mejorar el contexto final, pero añade inferencia y latencia.

[Opciones de consulta y compatibilidad](https://docs.aws.amazon.com/bedrock/latest/userguide/kb-test-config.html).

## Ejemplo conceptual y decisiones operativas

Un asistente de garantías ingiere manuales con `pais=CL`, `producto=X` y `version=2026`. Para «¿cubre el error E-104?», combina texto y semántica si el backend lo permite, filtra el producto y recupera candidatos antes de reranking. La aplicación valida las citas y la vigencia antes de presentar una respuesta. El filtro de país mejora relevancia; el acceso a documentos debe además corresponder a [[ACLs]] y a la identidad del usuario.

Evalúa preguntas con códigos, excepciones y documentos actualizados usando [[Golden dataset]] y [[RAG evals]]. Mide por separado recuperación y generación: una respuesta fluida puede ocultar un fragmento incorrecto. La utilidad de cada ajuste se decide con evidencia, no por aumentar todos los parámetros.

## Diferencia frente a las otras opciones

[[Vertex AI Vector Search]] clásico gestiona principalmente índices y vecinos; debes preparar el pipeline alrededor. [[Azure AI Search]] es un motor de búsqueda con esquema, texto, vectores y enriquecimiento opcional. Knowledge Bases agrega una capa de RAG y puede incluir generación; sus modalidades tampoco son equivalentes entre sí.

## Fuentes oficiales

- [Documentación oficial](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base.html). Consulta: 2026-09-30.
