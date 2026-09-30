---
tipo: "concepto"
dominio: "clouds"
bloque: "08 · Observabilidad"
orden: 810
estado: "por-ver"
parent: "[[Clouds para Gen AI]]"
prereqs: ["[[Clouds para Gen AI]]"]
se_evalua_con: []
contrasta_con: ["[[Google Cloud Observability]]", "[[Azure Monitor]]"]
fuentes: ["https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html"]
---
# Amazon CloudWatch

![[assets/clouds/cloudwatch.svg|72]]

**Qué es:** Servicio de AWS que reúne señales operativas y permite consultarlas, crear dashboards y activar alarmas.

**Para qué sirve:** Sirve para detectar errores y cambios de rendimiento mediante métricas y logs. En una API RAG, la instrumentación registra latencia de recuperación y generación, errores de herramientas y tokens usados; una alarma avisa cuando aumenta la tasa de fallos.

**Clave:** Las métricas describen la ejecución; evaluar la calidad de las respuestas requiere casos esperados y criterios propios.

[[Google Cloud Observability]] · [[Azure Monitor]] · [[Clouds para Gen AI]]

[Documentación oficial de Amazon CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html). Consultada el 30 de septiembre de 2026.
