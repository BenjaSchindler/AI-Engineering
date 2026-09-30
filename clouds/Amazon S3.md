---
tipo: "concepto"
dominio: "clouds"
bloque: "04 · Almacenamiento"
orden: 410
estado: "por-ver"
parent: "[[Clouds para Gen AI]]"
prereqs: ["[[Clouds para Gen AI]]"]
se_evalua_con: []
contrasta_con: ["[[Cloud Storage]]", "[[Azure Blob Storage]]"]
fuentes: ["https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html"]
---
# Amazon S3

![[assets/clouds/s3.svg|72]]

**Qué es:** Almacenamiento de objetos de AWS que conserva archivos y datos dentro de buckets.

**Para qué sirve:** Sirve para guardar documentos originales, datasets y resultados con permisos y versiones. En un RAG, una notificación configurada en el bucket inicia la ingesta al subir un PDF; cada fragmento conserva la referencia y versión del archivo para rastrear su origen.

**Clave:** Un bucket guarda archivos; la búsqueda semántica requiere extraer fragmentos y crear un índice aparte.

[[Cloud Storage]] · [[Azure Blob Storage]] · [[Clouds para Gen AI]]

[Documentación oficial de Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html). Consultada el 30 de septiembre de 2026.
