---
tipo: concepto
dominio: clouds
bloque: "02 · Agentes"
orden: 230
estado: por-ver
parent: "[[Clouds para Gen AI]]"
prereqs: ["[[Tools y function calling]]", "[[Contexto, memoria y estado]]"]
se_evalua_con: ["[[Golden cases agénticos]]"]
contrasta_con: ["[[Amazon Bedrock AgentCore]]", "[[Vertex AI Agent Engine]]"]
fuentes: ["https://learn.microsoft.com/en-us/azure/foundry/agents/overview?view=foundry"]
aliases: ["Azure AI Agent Service"]
---
# Microsoft Foundry Agent Service

## En el mapa

### Microsoft Foundry Agent Service

![[assets/clouds/foundry-agents.svg|72]]

**Qué es:** Servicio de Azure para construir y operar agentes con modelos, herramientas y ejecución gestionada.

**Para qué sirve:** Permite configurar agentes declarativos mediante instrucciones y herramientas, o alojar agentes con código propio. Por ejemplo, un asistente busca la política de garantía, consulta el estado de una compra y propone la siguiente acción con los datos recuperados.

**Clave:** Es la capa de agentes de Microsoft Foundry; elegir entre agentes declarativos y alojados determina cuánto control tenés sobre la lógica de ejecución.

[[Amazon Bedrock AgentCore]] · [[Vertex AI Agent Engine]] · [[Clouds para Gen AI]]

## Qué se puede configurar

Primero elegí cuánto código querés administrar. Un **prompt agent** se define con instrucciones, modelo y herramientas; un **Hosted agent** ejecuta tu aplicación. También podés invocar Responses API desde una aplicación alojada fuera de Foundry. Estas rutas distribuyen de forma distinta la responsabilidad de orquestación. [Opciones de construcción](https://learn.microsoft.com/en-us/azure/foundry/agents/overview?view=foundry).

| Capa | Qué elegís | Efecto y límite |
| --- | --- | --- |
| Agente declarativo | Modelo compatible, instrucciones y herramientas | El servicio ejecuta el agente; la compatibilidad de herramientas depende del modelo y ruta elegidos. |
| Código propio | Imagen, framework y configuración de una versión Hosted | Controlás el flujo y empaquetado; debés cumplir el contrato de hosting. |
| Recursos Hosted | CPU/memoria por sesión y timeout de inactividad | Afecta capacidad por usuario y reanudación; escala por sesión, sin cantidad de réplicas configurable. |
| Protocolo Hosted | Responses o Invocations, incluidos modos de streaming | Responses administra historial; Invocations permite payload propio y exige gestionar el historial en código. |
| Herramientas | Toolbox compartido con MCP y mecanismos de autenticación | Centraliza exposición y gobierno; el backend debe validar parámetros y permisos. |
| Identidad | Identidad Entra del agente y permisos sobre recursos | Habilita acceso autenticado; recursos externos requieren permisos adicionales. |
| Observación | Trazas, métricas, evaluaciones y Application Insights | Ayuda a explicar resultados y medir calidad; necesitás casos y umbrales propios. |

Fuentes: [componentes y Toolbox](https://learn.microsoft.com/en-us/azure/foundry/agents/overview?view=foundry), [contrato y controles de Hosted agents](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/hosted-agents).

## Diferencias que cambian la decisión

- Frente a [[Amazon Bedrock AgentCore]]: comparar un prompt agent con Runtime mezcla una definición gestionada con hosting de código. Hosted agents se acercan a AgentCore Runtime; AgentCore además permite consumir capacidades independientes y ofrece Harness para un bucle gestionado.
- Frente a [[Vertex AI Agent Engine]]: Google permite ajustar mínimo/máximo de instancias y concurrencia del contenedor mediante controles personalizados documentados como Preview. Foundry Hosted usa cómputo por sesión. En la documentación consultada, el endpoint Hosted sirve una versión a la vez y no divide tráfico entre versiones.

Referencias: [AgentCore modular](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/what-is-bedrock-agentcore.html), [capacidad de Google](https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale/runtime/deploy-an-agent) y [versiones de Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/hosted-agents).

## Ejemplo de decisiones

**Configuración conceptual, no ejecutable:** empezá con un prompt agent para buscar garantías y consultar pedidos mediante herramientas. Elegí Hosted si necesitás un flujo propio con checkpoints y reintentos específicos. Usá identidad con lectura de pedidos, una herramienta de propuesta de devolución y aprobación comercial antes del reembolso. Compará ambas rutas con [[Golden cases agénticos]].

Una conversación persistida no equivale a extracción automática de preferencias. Para RAG, la herramienta puede consultar [[Vector DB]] o un servicio de búsqueda: configurá esa recuperación y sus permisos por separado. Hosting, modelos y herramientas conectadas requieren verificar disponibilidad y costos de cada componente.

## Fuentes oficiales

- [Documentación oficial](https://learn.microsoft.com/en-us/azure/foundry/agents/overview?view=foundry). Consulta: 2026-09-30.
