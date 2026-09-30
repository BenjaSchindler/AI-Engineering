---
tipo: "concepto"
dominio: "clouds"
bloque: "05 · Contenedores"
orden: 530
estado: "por-ver"
parent: "[[Clouds para Gen AI]]"
prereqs: ["[[Clouds para Gen AI]]"]
se_evalua_con: []
contrasta_con: ["[[Amazon ECS con AWS Fargate]]", "[[Cloud Run]]"]
fuentes: ["https://learn.microsoft.com/en-us/azure/container-apps/overview"]
---
# Azure Container Apps

![[assets/clouds/container-apps.svg|72]]

**Qué es:** Plataforma gestionada de Azure para desplegar aplicaciones y jobs basados en contenedores.

**Para qué sirve:** Sirve para ejecutar APIs y procesos con escalado según tráfico o eventos. En un agente Gen AI, una aplicación atiende conversaciones y un job procesa documentos; las revisiones permiten probar una nueva imagen y observar su comportamiento antes de ampliar el tráfico.

**Clave:** La aplicación atiende solicitudes; el job ejecuta trabajo finito con su propio inicio y finalización.

[[Amazon ECS con AWS Fargate]] · [[Cloud Run]] · [[Clouds para Gen AI]]

[Documentación oficial de Azure Container Apps](https://learn.microsoft.com/en-us/azure/container-apps/overview). Consultada el 30 de septiembre de 2026.
