---
tipo: "concepto"
dominio: "clouds"
bloque: "04 · Almacenamiento"
orden: 430
estado: "por-ver"
parent: "[[Clouds para Gen AI]]"
prereqs: ["[[Clouds para Gen AI]]"]
se_evalua_con: []
contrasta_con: ["[[Amazon S3]]", "[[Cloud Storage]]"]
fuentes: ["https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blobs-overview"]
---
# Azure Blob Storage

![[assets/clouds/blob-storage.svg|72]]

**Qué es:** Almacenamiento de objetos de Azure para datos no estructurados, organizado en cuentas, contenedores y blobs.

**Para qué sirve:** Sirve para conservar documentos, imágenes y archivos derivados con niveles de acceso y opciones de redundancia. En un RAG de contratos, la ingesta lee blobs privados y conserva la dirección y versión de cada documento junto a sus fragmentos.

**Clave:** Un contenedor de almacenamiento agrupa archivos; el índice de búsqueda se construye en otro componente.

[[Amazon S3]] · [[Cloud Storage]] · [[Clouds para Gen AI]]

[Documentación oficial de Azure Blob Storage](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blobs-overview). Consultada el 30 de septiembre de 2026.
