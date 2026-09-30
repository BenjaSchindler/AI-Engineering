---
tipo: "concepto"
dominio: "clouds"
bloque: "06 · Funciones"
orden: 630
estado: "por-ver"
parent: "[[Clouds para Gen AI]]"
prereqs: ["[[Clouds para Gen AI]]"]
se_evalua_con: []
contrasta_con: ["[[AWS Lambda]]", "[[Cloud Run functions]]"]
fuentes: ["https://learn.microsoft.com/en-us/azure/azure-functions/functions-overview"]
---
# Azure Functions

![[assets/clouds/azure-functions.svg|72]]

**Qué es:** Plataforma de Azure para ejecutar funciones activadas por eventos y conectar servicios mediante triggers y bindings.

**Para qué sirve:** Sirve para responder a HTTP, temporizadores o mensajes con entradas y salidas de datos integradas. En un RAG, una función recibe la referencia de un blob, valida sus metadatos, solicita embeddings y registra el estado de ingesta.

**Clave:** El plan de alojamiento determina recursos y escalado; la idempotencia del proceso evita duplicados cuando se reintenta un evento.

[[AWS Lambda]] · [[Cloud Run functions]] · [[Clouds para Gen AI]]

[Documentación oficial de Azure Functions](https://learn.microsoft.com/en-us/azure/azure-functions/functions-overview). Consultada el 30 de septiembre de 2026.
