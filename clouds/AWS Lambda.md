---
tipo: "concepto"
dominio: "clouds"
bloque: "06 · Funciones"
orden: 610
estado: "por-ver"
parent: "[[Clouds para Gen AI]]"
prereqs: ["[[Clouds para Gen AI]]"]
se_evalua_con: []
contrasta_con: ["[[Cloud Run functions]]", "[[Azure Functions]]"]
fuentes: ["https://docs.aws.amazon.com/lambda/latest/dg/welcome.html"]
---
# AWS Lambda

![[assets/clouds/lambda.svg|72]]

**Qué es:** Servicio de AWS que ejecuta funciones activadas por solicitudes o eventos con infraestructura gestionada.

**Para qué sirve:** Sirve para validar datos, transformar información y conectar servicios mediante código. En un RAG, un trigger configurado para subidas a S3 activa una función que comprueba el formato del PDF y envía su referencia a una cola de ingesta para generar embeddings.

**Clave:** Aunque admita imágenes de contenedor, la función sigue el contrato de ejecución y los límites de Lambda.

[[Cloud Run functions]] · [[Azure Functions]] · [[Clouds para Gen AI]]

[Documentación oficial de AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html). Consultada el 30 de septiembre de 2026.
