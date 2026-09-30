---
tipo: "concepto"
dominio: "clouds"
bloque: "06 · Funciones"
orden: 620
estado: "por-ver"
parent: "[[Clouds para Gen AI]]"
prereqs: ["[[Clouds para Gen AI]]"]
se_evalua_con: []
contrasta_con: ["[[AWS Lambda]]", "[[Azure Functions]]"]
fuentes: ["https://docs.cloud.google.com/run/docs/functions/overview"]
---
# Cloud Run functions

![[assets/clouds/cloud-functions.svg|72]]

**Qué es:** Experiencia de Google Cloud para desplegar funciones desde código y ejecutarlas sobre Cloud Run.

**Para qué sirve:** Sirve para responder a HTTP y eventos mediante una función que el build gestionado convierte en imagen. En un RAG, un trigger de Eventarc configurado para documentos nuevos activa la extracción de metadatos y el envío del trabajo de embeddings a una cola.

**Clave:** Las funciones actuales y la primera generación tienen diferencias de ejecución y APIs que importan al comparar límites o migrar.

[[AWS Lambda]] · [[Azure Functions]] · [[Clouds para Gen AI]]

[Documentación oficial de Cloud Run functions](https://docs.cloud.google.com/run/docs/functions/overview). Consultada el 30 de septiembre de 2026.
