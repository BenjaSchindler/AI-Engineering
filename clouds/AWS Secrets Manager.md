---
tipo: "concepto"
dominio: "clouds"
bloque: "07 · Secretos"
orden: 710
estado: "por-ver"
parent: "[[Clouds para Gen AI]]"
prereqs: ["[[Clouds para Gen AI]]"]
se_evalua_con: []
contrasta_con: ["[[Secret Manager (GCP)]]", "[[Azure Key Vault]]"]
fuentes: ["https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html"]
---
# AWS Secrets Manager

![[assets/clouds/secrets-manager.svg|72]]

**Qué es:** Servicio de AWS para guardar, recuperar y administrar credenciales y otros secretos de aplicaciones.

**Para qué sirve:** Sirve para centralizar valores sensibles con permisos IAM, versiones y mecanismos de rotación. En un pipeline Gen AI, una tarea de ingesta usa su rol para obtener la clave de una API externa al arrancar, sin incluirla en la imagen del contenedor.

**Clave:** El secreto contiene una credencial; el rol identifica a la tarea y determina si puede leerla.

[[Secret Manager (GCP)]] · [[Azure Key Vault]] · [[Clouds para Gen AI]]

[Documentación oficial de AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html). Consultada el 30 de septiembre de 2026.
