---
tipo: mapa
dominio: clouds
estado: por-ver
prereqs: ["[[Fundamentos]]", "[[RAG básico]]"]
se_evalua_con: ["[[RAG evals]]", "[[Evals de rendimiento]]"]
contrasta_con: []
aliases: ["AWS, GCP y Azure", "Clouds", "Gen AI en la nube"]
fuentes:
  - https://aws.amazon.com/bedrock/
  - https://cloud.google.com/products/gemini-enterprise-agent-platform
  - https://learn.microsoft.com/en-us/azure/foundry/what-is-foundry
---
# Clouds para Gen AI

## Resumen

> Servicios de AWS, Google Cloud y Azure para construir y ejecutar aplicaciones de IA generativa.

[[Clouds para Gen AI.canvas|Abrir el submapa de los tres clouds]] · [[Mapa.canvas|Volver al mapa principal]]

![[assets/clouds-comparacion.svg|1400]]

## Cómo usarlo

El esquema completo está en [[Clouds para Gen AI.canvas|su propio submapa]], accesible desde la sección **04 · Clouds para Gen AI** de [[Mapa.canvas]]. El primer nodo reúne este resumen y la imagen. Debajo están los servicios en tres columnas: **AWS naranja · GCP verde · Azure azul**. Cada ficha explica **qué es**, **para qué sirve** y una diferencia clave.

Leé una fila para comparar una función o bajá por una columna para estudiar un cloud.

La profundidad de **agentes** y **recuperación RAG** se lee directamente en el mismo submapa: cada tema tiene filas adicionales con controles, efectos y diferencias. Al final, cuatro tarjetas explican parámetros de Vector DB, HNSW e IVF, alternativas y un ejemplo. Las notas conservan el desarrollo y las fuentes.

## Servicios por función

| Función | AWS | GCP | Azure |
|---|---|---|---|
| Generar con modelos | [[Amazon Bedrock]] | [[Vertex AI]] | [[Microsoft Foundry]] |
| Ejecutar agentes | [[Amazon Bedrock AgentCore]] | [[Vertex AI Agent Engine]] | [[Microsoft Foundry Agent Service]] |
| Buscar evidencia · RAG | [[Amazon Bedrock Knowledge Bases]] | [[Vertex AI Vector Search]] | [[Azure AI Search]] |
| Guardar archivos | [[Amazon S3]] | [[Cloud Storage]] | [[Azure Blob Storage]] |
| Ejecutar contenedores | [[Amazon ECS con AWS Fargate]] | [[Cloud Run]] | [[Azure Container Apps]] |
| Responder a eventos | [[AWS Lambda]] | [[Cloud Run functions]] | [[Azure Functions]] |
| Guardar secretos | [[AWS Secrets Manager]] | [[Secret Manager (GCP)]] | [[Azure Key Vault]] |
| Observar la aplicación | [[Amazon CloudWatch]] | [[Google Cloud Observability]] | [[Azure Monitor]] |

## Tres conceptos para empezar

- **Bucket:** contenedor de archivos u objetos. Sirve para guardar los documentos originales de un RAG.
- **Contenedor de aplicación:** paquete de código y dependencias. Sirve para desplegar una API o un proceso de ingesta.
- **Runtime de agentes:** entorno donde corre el agente y usa herramientas. Sirve para ejecutar la lógica que rodea al modelo.

**Ejemplo:** PDF → almacenamiento → ingesta e índice → recuperación → modelo → respuesta.

Las filas comparan funciones; el alcance de los productos varía. Cada ficha aclara la diferencia y conserva sus fuentes.

## Agentes - qué los diferencia

**La primera decisión:** cuánto del comportamiento del agente definís con configuración y cuánto programás vos.

| Decisión | AWS · AgentCore | GCP · Agent Runtime | Azure · Foundry Agent Service |
|---|---|---|---|
| Unidad principal | Componentes que podés combinar: Runtime, Memory, Gateway, Identity… | Runtime con servicios de Sessions y Memory Bank | Agentes definidos por configuración o agentes con código propio |
| Ejecutar tu código | Runtime hospeda la lógica de tu framework | Desplegás código, un objeto de agente o un contenedor | Hosted agent ejecuta tu código en un contenedor |
| Delegar el bucle del agente | Harness recibe modelo, instrucciones y herramientas | El bucle depende de la forma de construcción elegida; Runtime hospeda la aplicación | Prompt agent recibe instrucciones, modelo y herramientas |
| Conservar contexto | Memory es un componente que integrás | Sessions conserva interacciones; Memory Bank, recuerdos entre sesiones | Conversaciones y herramientas de memoria según el tipo de agente |
| Conectar herramientas | Gateway expone herramientas MCP; Identity gestiona credenciales | Herramientas del agente y conectividad gobernada mediante Agent Gateway | Tools y Toolboxes agrupan herramientas, conexiones y autenticación |

Fuentes de la comparación: [componentes de AgentCore](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/what-is-bedrock-agentcore.html), [Harness](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/harness.html), [servicios de GCP](https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale), [despliegue en GCP](https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale/runtime/deploy-an-agent) y [tipos de agentes de Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/overview).

**Qué comparar en un caso real:** herramientas permitidas, identidad del usuario, memoria que persiste, red privada, capacidad y trazas. La disponibilidad concreta depende del componente y del modo de despliegue.

Configuración detallada: [[Amazon Bedrock AgentCore#Qué se puede configurar|AgentCore]] · [[Vertex AI Agent Engine#Qué se puede configurar|Agent Runtime / Agent Engine]] · [[Microsoft Foundry Agent Service#Qué se puede configurar|Foundry Agent Service]].

## VectorDB y recuperación - qué los diferencia

**Una base vectorial guarda y consulta vectores; un servicio RAG puede encargarse además de preparar documentos y generar respuestas.** Esa diferencia cambia qué ajustes quedan en tus manos.

| Decisión | AWS · Bedrock Knowledge Bases | GCP · Vector Search clásico | Azure · AI Search |
|---|---|---|---|
| Qué contratás | Flujo de conocimiento y recuperación, con dos modalidades | Índice y servicio de búsqueda vectorial | Motor de búsqueda textual, vectorial e híbrida |
| Quién prepara los datos | Ingesta gestionada; el control varía entre Managed y Customer-managed | Tu pipeline prepara los embeddings que indexás | Tu pipeline o indexadores con habilidades de procesamiento |
| Qué ajustás en el índice | En Customer-managed elegís un almacén compatible; Managed administra la infraestructura | Dimensión, distancia, algoritmo, actualización y despliegue | Campos, perfiles vectoriales, HNSW o búsqueda exhaustiva |
| Cómo buscás | Configuración de recuperación; filtros y reranking según modalidad | Vecinos, filtros y búsqueda híbrida densa + dispersa | Consulta textual/vectorial, filtros y fusión híbrida; ranking semántico opcional |
| Qué falta para responder | Podés recuperar evidencia o usar generación integrada | Tu aplicación combina resultados, contexto y modelo | Tu aplicación integra la recuperación con el modelo; hay opciones de recuperación agéntica |

Fuentes de la comparación: [modalidades de Knowledge Bases](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base.html), [Vector Search](https://docs.cloud.google.com/gemini-enterprise-agent-platform/build/vector-search/overview) y [búsqueda vectorial en Azure AI Search](https://learn.microsoft.com/en-us/azure/search/vector-search-overview). Los parámetros y sus fuentes específicas están en cada ficha.

**Para estudiar:** [[Vector DB]] explica dimensión, distancia, `top-k`, filtros y el costo de mejorar el recall. [[ANN HNSW]] y [[ANN IVF]] explican los algoritmos. [[Vector DB#Diferencias entre implementaciones|Comparativa de bases vectoriales]] compara pgvector, Qdrant, Pinecone y Milvus.

Configuración por cloud: [[Amazon Bedrock Knowledge Bases#Qué se puede configurar|Knowledge Bases]] · [[Vertex AI Vector Search#Qué se puede configurar|Vector Search]] · [[Azure AI Search#Qué se puede configurar|Azure AI Search]].

> Nombres revisados el **30 de septiembre de 2026**: Agent Engine aparece como **Agent Runtime** en la documentación actual de GCP. **Agent Retrieval**, antes Vector Search 2.0, tiene un alcance distinto del Vector Search clásico comparado aquí. Las notas conservan los nombres conocidos para facilitar la navegación.

## Progreso

```dataview
TABLE WITHOUT ID file.link AS servicio, bloque, estado
FROM "clouds" WHERE tipo != "mapa" SORT orden
```

[[RAG]] · [[Runtime de agentes]] · [[Seguridad]] · [[Evals]] · [[assets/clouds/FUENTES|Fuentes de los iconos]]
