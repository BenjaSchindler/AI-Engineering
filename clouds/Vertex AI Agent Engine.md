---
tipo: concepto
dominio: clouds
bloque: "02 · Agentes"
orden: 220
estado: por-ver
parent: "[[Clouds para Gen AI]]"
prereqs: ["[[Tools y function calling]]", "[[Contexto, memoria y estado]]"]
se_evalua_con: ["[[Golden cases agénticos]]"]
contrasta_con: ["[[Amazon Bedrock AgentCore]]", "[[Microsoft Foundry Agent Service]]"]
fuentes: ["https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale", "https://docs.cloud.google.com/gemini-enterprise-agent-platform/release-notes"]
aliases: ["Agent Runtime", "Google Cloud Agent Runtime"]
---
# Vertex AI Agent Engine

## En el mapa

### Vertex AI Agent Engine

![[assets/clouds/agent-engine.svg|72]]

**Qué es:** Servicio de Google Cloud para hospedar y escalar aplicaciones de agentes con ejecución gestionada.

**Para qué sirve:** Permite desplegar un agente desarrollado localmente y operar sus conversaciones con servicios como Sessions y Memory Bank. Por ejemplo, un agente construido con ADK consulta pedidos y conserva el contexto cuando el cliente pregunta por una devolución en el siguiente turno.

**Clave:** Vertex AI Agent Engine ahora se llama Agent Runtime dentro de Gemini Enterprise Agent Platform; la lógica y las herramientas siguen dependiendo de tu aplicación.

[[Amazon Bedrock AgentCore]] · [[Microsoft Foundry Agent Service]] · [[Clouds para Gen AI]]

## Qué se puede configurar

El nombre actual es **Agent Runtime** dentro de Gemini Enterprise Agent Platform; se mantiene el nombre de esta nota para preservar enlaces. Sessions y Memory Bank complementan el hosting con estado conversacional y memoria entre sesiones. [Mapa de servicios](https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale).

| Capa | Qué elegís | Efecto y límite |
| --- | --- | --- |
| Aplicación | Framework, instrucciones, modelo y herramientas en tu agente | ADK tiene integración documentada; también se admite código propio. La lógica comercial sigue en la aplicación. |
| Despliegue | Objeto de agente, fuentes, repositorio, Dockerfile o imagen | Las rutas de contenedor permiten otros lenguajes, respetando el contrato del runtime; las rutas de fuentes/objeto tienen requisitos propios. |
| Capacidad | `min_instances`, `max_instances`, CPU, memoria y `container_concurrency` | Ajusta capacidad y arranque. Estos controles personalizados figuran como Preview en la guía consultada. |
| Entorno | Dependencias, variables y referencias a Secret Manager | Hace reproducible la ejecución; las referencias de secretos documentadas requieren el mismo proyecto y permisos. |
| Acceso y red | Identidad del agente o cuenta de servicio, IAM y PSC | Conecta recursos privados y limita acceso; configurar red no sustituye autorización por usuario. |
| Contexto | Sessions y Memory Bank | Guarda turnos y recupera hechos/preferencias persistentes; la aplicación decide cuándo usarlos. |
| Operación | Cloud Trace, Logging, métricas y evaluación | Permite seguir llamadas y detectar fallos; requiere instrumentación y criterios de calidad. |

Referencias: [despliegue y parámetros](https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale/runtime/deploy-an-agent), [servicios de contexto y operación](https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale).

## Cómo afectan los controles de capacidad

Elevar el mínimo de instancias reduce la probabilidad de esperar un arranque; elevar concurrencia aprovecha mejor agentes asíncronos que esperan modelos o APIs. Demasiada concurrencia puede agotar memoria. Medí con tráfico representativo antes de fijar valores: los resultados de un ejemplo oficial no garantizan tu latencia. [Guía de rendimiento](https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale/runtime/optimize-and-scale).

## Diferencias que cambian la decisión

- Frente a [[Amazon Bedrock AgentCore]]: Agent Runtime expone controles de capacidad de contenedor; AgentCore Runtime distingue sesiones microVM e Instances. AgentCore es un conjunto modular más amplio que su Runtime; compará cada capacidad concreta.
- Frente a [[Microsoft Foundry Agent Service]]: Agent Runtime hospeda la aplicación que desplegás. Foundry también ofrece prompt agents declarativos; sus Hosted agents son la comparación más cercana para código propio y escalan por sesión.

Fuentes de comparación: [AWS Runtime](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/agents-tools-runtime.html) y [Hosted agents](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/hosted-agents).

## Ejemplo de decisiones

**Configuración conceptual, no ejecutable:** agente ADK de soporte con Sessions por conversación, Memory Bank para idioma preferido, identidad con acceso de lectura a pedidos y una herramienta para proponer devoluciones. Ajustá concurrencia después de medir espera de APIs y consumo de memoria; validá cambios con [[Golden cases agénticos]].

La recuperación documental mediante [[Vector DB]] requiere una integración adicional. Elegí por separado región y controles de cada servicio: la matriz oficial de seguridad difiere entre Runtime, Memory Bank y Example Store. [Seguridad por servicio](https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale).

## Fuentes oficiales

- [Documentación oficial](https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale). Consulta: 2026-09-30.
- [Notas oficiales: Agent Engine pasa a Agent Runtime](https://docs.cloud.google.com/gemini-enterprise-agent-platform/release-notes).
