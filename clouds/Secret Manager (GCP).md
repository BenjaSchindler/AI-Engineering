---
tipo: "concepto"
dominio: "clouds"
bloque: "07 · Secretos"
orden: 720
estado: "por-ver"
parent: "[[Clouds para Gen AI]]"
prereqs: ["[[Clouds para Gen AI]]"]
se_evalua_con: []
contrasta_con: ["[[AWS Secrets Manager]]", "[[Azure Key Vault]]"]
fuentes: ["https://docs.cloud.google.com/secret-manager/docs/overview"]
---
# Secret Manager (GCP)

![[assets/clouds/secret-manager.svg|72]]

**Qué es:** Servicio de Google Cloud para almacenar secretos con versiones y permisos de IAM.

**Para qué sirve:** Sirve para centralizar claves de API y credenciales que las aplicaciones recuperan con una identidad autorizada. En un agente Gen AI, Cloud Run usa su cuenta de servicio para leer la clave del proveedor de modelos y comprueba una nueva versión antes de retirar la anterior.

**Clave:** Versionar un secreto cambia su valor disponible; IAM determina quién puede acceder a él.

[[AWS Secrets Manager]] · [[Azure Key Vault]] · [[Clouds para Gen AI]]

[Documentación oficial de Secret Manager (GCP)](https://docs.cloud.google.com/secret-manager/docs/overview). Consultada el 30 de septiembre de 2026.
