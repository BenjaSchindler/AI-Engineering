---
tipo: "concepto"
dominio: "clouds"
bloque: "07 · Secretos"
orden: 730
estado: "por-ver"
parent: "[[Clouds para Gen AI]]"
prereqs: ["[[Clouds para Gen AI]]"]
se_evalua_con: []
contrasta_con: ["[[AWS Secrets Manager]]", "[[Secret Manager (GCP)]]"]
fuentes: ["https://learn.microsoft.com/en-us/azure/key-vault/general/overview"]
---
# Azure Key Vault

![[assets/clouds/key-vault.svg|72]]

**Qué es:** Servicio de Azure para administrar secretos, claves criptográficas y certificados con control de acceso.

**Para qué sirve:** Sirve para guardar credenciales y material criptográfico fuera del código y las imágenes. En un agente Gen AI, Container Apps usa una identidad administrada para obtener la credencial del proveedor de embeddings y el equipo comprueba qué versión consume tras actualizarla.

**Clave:** La identidad autoriza el acceso; Key Vault guarda el secreto y también gestiona claves y certificados.

[[AWS Secrets Manager]] · [[Secret Manager (GCP)]] · [[Clouds para Gen AI]]

[Documentación oficial de Azure Key Vault](https://learn.microsoft.com/en-us/azure/key-vault/general/overview). Consultada el 30 de septiembre de 2026.
