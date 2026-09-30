---
tipo: concepto
dominio: clouds
bloque: "02 · Agentes"
orden: 210
estado: por-ver
parent: "[[Clouds para Gen AI]]"
prereqs: ["[[Tools y function calling]]", "[[Contexto, memoria y estado]]"]
se_evalua_con: ["[[Golden cases agénticos]]"]
contrasta_con: ["[[Vertex AI Agent Engine]]", "[[Microsoft Foundry Agent Service]]"]
fuentes: ["https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/what-is-bedrock-agentcore.html"]
aliases: []
---
# Amazon Bedrock AgentCore

## En el mapa

### Amazon Bedrock AgentCore

![[assets/clouds/agentcore.svg|72]]

**Qué es:** Conjunto de servicios gestionados de AWS para ejecutar, conectar y observar agentes en producción.

**Para qué sirve:** Runtime hospeda el agente, mientras otros componentes aportan memoria, identidad y acceso a herramientas. Por ejemplo, un agente de soporte consulta una orden mediante una herramienta, conserva contexto entre interacciones y prepara una devolución según las políticas comerciales.

**Clave:** AgentCore es modular y admite distintos frameworks y modelos; la comparación con otros servicios de agentes se centra principalmente en su Runtime.

[[Vertex AI Agent Engine]] · [[Microsoft Foundry Agent Service]] · [[Clouds para Gen AI]]

## Qué se puede configurar

AgentCore separa la ejecución de las capacidades auxiliares. Elegí los componentes según el problema; desplegar Runtime no conecta automáticamente memoria, herramientas ni políticas. [Arquitectura modular](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/what-is-bedrock-agentcore.html).

| Capa | Qué elegís | Efecto y límite |
| --- | --- | --- |
| Aplicación | Framework, modelo, instrucciones y lógica de herramientas en tu código; Harness ofrece un bucle gestionado como alternativa | Controlás cómo decide el agente; un runtime por sí solo no diseña el flujo comercial. |
| Runtime | Artefacto, protocolo, `runtimeSessionId`, tipo de cómputo, `idleRuntimeSessionTimeout` y `maxLifetime` | MicroVM aísla cada sesión; el timeout de inactividad libera cómputo y el máximo limita su vida. Instances usa EC2 gestionado en tu cuenta mediante un capacity provider, con responsabilidades de seguridad distintas. [Ciclo de vida](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/runtime-lifecycle-settings.html) · [Instances](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/runtime-instances.html). |
| Identidad | Entrada IAM SigV4 o JWT; para JWT, discovery OIDC, audiences, clients, scopes y claims. Rol IAM de ejecución y credenciales de salida | El authorizer controla invocación; el rol limita acceso AWS y Identity aporta OAuth/API keys para servicios externos. Son permisos distintos. [Entrada y salida](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/runtime-oauth.html) · [rol de ejecución](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/runtime-lifecycle-settings.html). |
| Memoria | Eventos de sesión y uso de memoria persistente | Recupera contexto entre turnos y sesiones; tu aplicación debe decidir qué información conservar y consultar. |
| Gateway | OpenAPI/Smithy, esquemas de Lambda o servidores MCP; proveedor de credenciales por destino | Elegís IAM, OAuth o API key según destino; para API key, header/query, nombre y prefijo. Las combinaciones admitidas dependen del tipo de target. [Fuentes](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway-building.html) · [credenciales](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway-building-adding-targets-authorization.html). |
| Calidad | Instrumentación, evaluadores y reglas Policy para llamadas a herramientas | Asociás un policy engine al Gateway y reglas sobre identidad, herramienta y parámetros; intercepta llamadas que pasan por ese Gateway. La API comercial sigue validando la operación. [Policy](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/policy.html). |
| Browser, opcional | Browser gestionado o personalizado; timeout, red, rol IAM y grabación en S3 para el personalizado | Permite operar sitios y observar sesiones; la grabación almacena actividad y necesita permisos de almacenamiento. [Browser](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/browser-tool.html). |
| Code Interpreter, opcional | Propiedades de sesión, duración y modo de red | Ejecuta código en sandbox; permitir internet cambia los destinos alcanzables y exige controlar qué datos recibe. [Code Interpreter](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/code-interpreter-tool.html). |

Fuentes por capa: [Runtime e identidad](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/agents-tools-runtime.html), [Memory](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/memory.html), [Gateway](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway.html) y [servicios de calidad y Policy](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/what-is-bedrock-agentcore.html).


## Memoria: decisiones concretas

| Control | Qué produce | Consecuencia |
| --- | --- | --- |
| Estrategias | Resumen de sesión, preferencias de usuario, memoria semántica de hechos o episódica | Podés combinar estrategias; sin ninguna conservás solo eventos crudos de corto plazo. [Estrategias](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/built-in-strategies.html). |
| `actorId`, `sessionId` y namespace | Por ejemplo, `/users/{actorId}/preferences/` y `/summaries/{actorId}/{sessionId}/` | Organiza registros por usuario y sesión; derivá actor de la identidad autenticada y aplicá permisos, porque un namespace no autoriza acceso. [Creación](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/memory-create-a-memory-store.html). |
| `eventExpiryDuration` | Retención de eventos crudos | Se configura en el almacén de memoria; cada evento conserva el plazo vigente al crearse. Cambiarlo afecta eventos nuevos y no extiende los anteriores. No equivale a borrar todos los recuerdos derivados. [Retención](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/memory-create-a-memory-store.html). |
| Cifrado | Clave gestionada por el servicio o propia en KMS | La clave propia añade control y gestión de permisos. [Creación y cifrado](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/memory-create-a-memory-store.html). |


## Diferencias que cambian la decisión

- Frente a [[Vertex AI Agent Engine]]: ambos alojan código de agentes, pero AgentCore presenta componentes independientes y una elección explícita entre microVM e Instances. Google ofrece parámetros personalizados de instancias y concurrencia del contenedor en Agent Runtime, documentados como Preview. No se traducen parámetro por parámetro.
- Frente a [[Microsoft Foundry Agent Service]]: Foundry permite definir prompt agents o alojar código; para comparar hosting, mirá Hosted agents. AgentCore Runtime deja el bucle a tu aplicación, mientras Harness es otra opción gestionada dentro del mismo conjunto.

Estas diferencias comparan las [opciones de AWS](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/agents-tools-runtime.html), el [despliegue de Google](https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale/runtime/deploy-an-agent) y las [rutas de Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/overview?view=foundry).

## Ejemplo de decisiones

**Configuración conceptual, no ejecutable:** agente de devoluciones con código propio en Runtime, sesión aislada por cliente, memoria para preferencias y Gateway con `consultar_pedido` y `solicitar_devolucion`. La herramienta verifica propietario y elegibilidad; una política limita acciones y el backend exige aprobación para emitir un reembolso. Medí éxito y llamadas incorrectas con [[Golden cases agénticos]].

La memoria aporta continuidad; una [[Vector DB]] para recuperar políticas de garantía resuelve otra necesidad. Modelos, almacenamiento y componentes conectados tienen sus propios requisitos y costos.

## Fuentes oficiales

- [Documentación oficial](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/what-is-bedrock-agentcore.html). Consulta: 2026-09-30.
