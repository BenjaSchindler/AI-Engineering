---
tipo: "concepto"
dominio: "clouds"
bloque: "05 · Contenedores"
orden: 510
estado: "por-ver"
parent: "[[Clouds para Gen AI]]"
prereqs: ["[[Clouds para Gen AI]]"]
se_evalua_con: []
contrasta_con: ["[[Cloud Run]]", "[[Azure Container Apps]]"]
fuentes: ["https://docs.aws.amazon.com/AmazonECS/latest/developerguide/AWS_Fargate.html"]
---
# Amazon ECS con AWS Fargate

![[assets/clouds/fargate.svg|72]]

**Qué es:** ECS orquesta tareas y servicios de contenedores; Fargate aporta el cómputo que los ejecuta sin administrar servidores.

**Para qué sirve:** Sirve para desplegar APIs y workers con imágenes propias, recursos y red configurados. En un RAG, un servicio ECS atiende preguntas y otras tareas consumen una cola para extraer documentos de S3 y producir embeddings.

**Clave:** ECS coordina el despliegue; Fargate ejecuta los contenedores y requiere configurar el diseño del servicio.

[[Cloud Run]] · [[Azure Container Apps]] · [[Clouds para Gen AI]]

[Documentación oficial de Amazon ECS con AWS Fargate](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/AWS_Fargate.html). Consultada el 30 de septiembre de 2026.
