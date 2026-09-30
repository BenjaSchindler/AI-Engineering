# Fuentes de los iconos cloud

Descargados el 30 de septiembre de 2026 desde los paquetes publicados por cada proveedor. Los SVG conservan sus bytes originales; solo se cambia el nombre de archivo. No son logos corporativos: son iconos oficiales de producto o arquitectura.

- [AWS Architecture Icons](https://aws.amazon.com/architecture/icons/): paquete 31 de julio de 2026.
- [Google Cloud Icon Library](https://cloud.google.com/icons): productos actuales y paquete legado para servicios sin icono en el paquete actual.
- [Azure Architecture Icons](https://learn.microsoft.com/en-us/azure/architecture/icons/): paquete V24.

## Paquetes originales

- [aws](https://d1.awsstatic.com/onedam/marketing-channels/website/public/shared/architecture-icon-release/Icon-package_07312026.5846e92413caa21490223536cc97f1269e44fa92.zip)
- [gcp](https://services.google.com/fh/files/misc/core-products-icons.zip)
- [gcp-legacy](https://services.google.com/fh/files/misc/google-cloud-legacy-icons.zip)
- [azure](https://arch-center.azureedge.net/icons/Azure_Public_Service_Icons_V24.zip)

## Correspondencias

| Archivo local | Paquete | Ruta original | Uso y fallback |
|---|---|---|---|
| `bedrock.svg` | aws | `Architecture-Service-Icons_07312026/Arch_Artificial-Intelligence/64/Arch_Amazon-Bedrock_64.svg` | Servicio correspondiente. |
| `agentcore.svg` | aws | `Architecture-Service-Icons_07312026/Arch_Artificial-Intelligence/64/Arch_Amazon-Bedrock-AgentCore_64.svg` | Servicio correspondiente. |
| `s3.svg` | aws | `Architecture-Service-Icons_07312026/Arch_Storage/64/Arch_Amazon-Simple-Storage-Service_64.svg` | Servicio correspondiente. |
| `fargate.svg` | aws | `Architecture-Service-Icons_07312026/Arch_Containers/64/Arch_AWS-Fargate_64.svg` | Servicio correspondiente. |
| `lambda.svg` | aws | `Architecture-Service-Icons_07312026/Arch_Compute/64/Arch_AWS-Lambda_64.svg` | Servicio correspondiente. |
| `secrets-manager.svg` | aws | `Architecture-Service-Icons_07312026/Arch_Security-Identity/64/Arch_AWS-Secrets-Manager_64.svg` | Servicio correspondiente. |
| `cloudwatch.svg` | aws | `Architecture-Service-Icons_07312026/Arch_Management-Tools/64/Arch_Amazon-CloudWatch_64.svg` | Servicio correspondiente. |
| `bedrock-knowledge-bases.svg` | aws | `Architecture-Service-Icons_07312026/Arch_Artificial-Intelligence/64/Arch_Amazon-Bedrock_64.svg` | Fallback: icono de Amazon Bedrock; el paquete no incluye un SVG dedicado a Knowledge Bases. |
| `vertex-ai.svg` | gcp | `Unique Icons/Vertex AI/SVG/VertexAI-512-color.svg` | Servicio correspondiente. |
| `cloud-storage.svg` | gcp | `Unique Icons/Cloud Storage/SVG/Cloud_Storage-512-color.svg` | Servicio correspondiente. |
| `cloud-run.svg` | gcp | `Unique Icons/Cloud Run/SVG/CloudRun-512-color-rgb.svg` | Servicio correspondiente. |
| `agent-engine.svg` | gcp | `Unique Icons/Vertex AI/SVG/VertexAI-512-color.svg` | Fallback: icono de Vertex AI, la familia del servicio; el paquete no incluye un SVG dedicado. |
| `vector-search.svg` | gcp | `Unique Icons/Vertex AI/SVG/VertexAI-512-color.svg` | Fallback: icono de Vertex AI, la familia del servicio; el paquete no incluye un SVG dedicado. |
| `cloud-functions.svg` | gcp-legacy | `cloud_functions/cloud_functions.svg` | Icono oficial del paquete legado de Google Cloud. |
| `secret-manager.svg` | gcp-legacy | `secret_manager/secret_manager.svg` | Icono oficial del paquete legado de Google Cloud. |
| `cloud-observability.svg` | gcp-legacy | `cloud_monitoring/cloud_monitoring.svg` | Fallback: icono oficial legado de Cloud Monitoring, componente de Google Cloud Observability. |
| `foundry.svg` | azure | `Azure_Public_Service_Icons/Icons/ai + machine learning/035746832-icon-service-AI-Foundry.svg` | Icono oficial.  |
| `foundry-agents.svg` | azure | `Azure_Public_Service_Icons/Icons/ai + machine learning/038470523-icon-service-Foundry-Agent-Service.svg` | Icono oficial.  |
| `ai-search.svg` | azure | `Azure_Public_Service_Icons/Icons/app services/10044-icon-service-Cognitive-Search.svg` | Icono oficial. Nombre de archivo histórico: Cognitive Search. |
| `blob-storage.svg` | azure | `Azure_Public_Service_Icons/Icons/general/10780-icon-service-Blob-Block.svg` | Icono oficial. Recurso Blob Block para Blob Storage. |
| `azure-functions.svg` | azure | `Azure_Public_Service_Icons/Icons/compute/10029-icon-service-Function-Apps.svg` | Icono oficial.  |
| `key-vault.svg` | azure | `Azure_Public_Service_Icons/Icons/security/10245-icon-service-Key-Vaults.svg` | Icono oficial.  |
| `azure-monitor.svg` | azure | `Azure_Public_Service_Icons/Icons/monitor/00001-icon-service-Monitor.svg` | Icono oficial.  |
| `container-apps.svg` | azure | `Azure_Public_Service_Icons/Icons/other/02884-icon-service-Worker-Container-App.svg` | Icono oficial Worker Container App para la aplicación en Azure Container Apps. |

Verificación: todos los SVG se parsean como XML, incluyen viewBox, no contienen script, foreignObject, manejadores de eventos ni referencias href externas. No se incluyen ZIP ni archivos del paquete que no se usan. Las marcas y derechos pertenecen a sus respectivos proveedores; consultar las condiciones de las páginas oficiales.
