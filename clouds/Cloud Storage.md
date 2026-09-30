---
tipo: "concepto"
dominio: "clouds"
bloque: "04 · Almacenamiento"
orden: 420
estado: "por-ver"
parent: "[[Clouds para Gen AI]]"
prereqs: ["[[Clouds para Gen AI]]"]
se_evalua_con: []
contrasta_con: ["[[Amazon S3]]", "[[Azure Blob Storage]]"]
fuentes: ["https://docs.cloud.google.com/storage/docs/introduction"]
---
# Cloud Storage

![[assets/clouds/cloud-storage.svg|72]]

**Qué es:** Almacenamiento de objetos de Google Cloud que organiza documentos y archivos en buckets.

**Para qué sirve:** Sirve para conservar fuentes, resultados de extracción y conjuntos de evaluación con políticas de acceso y retención. En un RAG, Cloud Run lee PDFs de un bucket, genera fragmentos y guarda un manifiesto que relaciona los resultados con sus documentos originales.

**Clave:** Guardar el corpus en un bucket no crea un índice para recuperar fragmentos por similitud.

[[Amazon S3]] · [[Azure Blob Storage]] · [[Clouds para Gen AI]]

[Documentación oficial de Cloud Storage](https://docs.cloud.google.com/storage/docs/introduction). Consultada el 30 de septiembre de 2026.
