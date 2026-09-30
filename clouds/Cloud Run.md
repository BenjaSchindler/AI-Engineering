---
tipo: "concepto"
dominio: "clouds"
bloque: "05 · Contenedores"
orden: 520
estado: "por-ver"
parent: "[[Clouds para Gen AI]]"
prereqs: ["[[Clouds para Gen AI]]"]
se_evalua_con: []
contrasta_con: ["[[Amazon ECS con AWS Fargate]]", "[[Azure Container Apps]]"]
fuentes: ["https://docs.cloud.google.com/run/docs/overview/what-is-cloud-run"]
---
# Cloud Run

![[assets/clouds/cloud-run.svg|72]]

**Qué es:** Plataforma gestionada de Google Cloud que ejecuta aplicaciones y tareas en contenedores.

**Para qué sirve:** Sirve para publicar APIs y procesar trabajos sin administrar un clúster. En un RAG, un servicio recibe preguntas por HTTP y un job independiente lee el corpus de Cloud Storage para reindexarlo con una imagen especializada.

**Clave:** El servicio atiende solicitudes y adapta su ejecución al tráfico; el job realiza tareas finitas hasta completarlas.

[[Amazon ECS con AWS Fargate]] · [[Azure Container Apps]] · [[Clouds para Gen AI]]

[Documentación oficial de Cloud Run](https://docs.cloud.google.com/run/docs/overview/what-is-cloud-run). Consultada el 30 de septiembre de 2026.
