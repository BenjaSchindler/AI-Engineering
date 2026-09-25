---
tipo: concepto
dominio: seguridad
estado: por-ver
parent: "[[Seguridad]]"
prereqs: ["[[Control humano y permisos]]"]
se_evalua_con: ["[[Tool evals]]", "[[Regresiones y CI]]"]
contrasta_con: ["[[Guardrails]]"]
fuentes:
  - https://www.anthropic.com/engineering/claude-code-sandboxing
bloque: "02 · Contención"
orden: 220
---
# Sandboxing y aislamiento

> **En una frase:** ejecutá código y herramientas dentro de límites que el modelo no pueda ampliar por sí mismo.

![Sandboxing: una inyección pide sacar la clave SSH y los límites de archivos y red lo impiden](../assets/sandboxing.svg)

## Cómo funciona
1. **Límites del sistema operativo, no del prompt.** El proceso corre aislado y las reglas las aplica el sistema: en Claude Code, bubblewrap en Linux y Seatbelt en macOS. Aunque el modelo obedezca una inyección, no puede ampliar esos límites.
2. **Archivos.** Lee lo autorizado y escribe solo en su directorio de trabajo; el resto del host, como `~/.ssh` o `.env`, queda fuera.
3. **Red.** La salida pasa por un proxy fuera del sandbox que solo permite ciertos dominios y pide confirmación para uno nuevo.
4. **Credenciales afuera.** El secreto no entra al sandbox: un proxy de credenciales verifica la operación y agrega un token acotado. En la versión web de Claude Code, así se manejan las credenciales de git.
5. **Recursos.** CPU, memoria, procesos y tiempo acotados, para que un script no tumbe el servidor.

Los dos primeros se necesitan juntos: sin aislamiento de red, un agente comprometido puede sacar archivos como las claves SSH; sin aislamiento de archivos, puede escapar del sandbox y conseguir red. Un contenedor con el directorio personal montado y red abierta no aporta esos límites por defecto. [Sandboxing de agentes](https://www.anthropic.com/engineering/claude-code-sandboxing).

## Qué limitar
| Límite | Ejemplo |
|---|---|
| Archivos | Leer datos autorizados y escribir solo en el directorio de trabajo |
| Red | Permitir únicamente los servicios necesarios; bloquear destinos internos ajenos a la tarea |
| Credenciales | Dar permisos mínimos y no exponer secretos al proceso que no los necesita |
| Recursos | Acotar CPU, memoria, procesos y tiempo de ejecución |

## Ejemplo
El agente lee una página que le ordena enviar `~/.ssh/id_rsa` a `evil.example`, y el script lo intenta. Leer la clave falla porque está fuera del directorio permitido; el envío falla porque el proxy no permite ese dominio. El mismo script sí puede hacer `git push`: sale por el proxy de credenciales, que agrega un token acotado sin que la clave entre al sandbox.

## Qué comprobar
- Probá lecturas, escrituras y conexiones prohibidas: deben fallar en la infraestructura, no depender de que el modelo se niegue.
- Separá las ejecuciones de distintos clientes y limpiá los datos temporales.
- Las acciones por APIs externas siguen necesitando permisos y aprobación cuando corresponda: estar dentro de un sandbox no las autoriza.

## Trampas
- **Aislar solo una cosa.** Archivos sin red, o red sin archivos, deja una vía de salida.
- **Variables de entorno heredadas.** El proceso hereda claves del servidor aunque el disco esté aislado. Arrancalo con un entorno limpio.
- **Lista de dominios amplia.** Permitir un dominio donde cualquiera puede subir contenido, como un almacenamiento público, abre una vía para sacar datos.
- **Confiar en el prompt.** “No envíes datos afuera” en las instrucciones no es un límite: una inyección puede anularlo. Los límites van en el sistema: [[Guardrails]].

> [!TIP] Para recordar
> **Archivos y red, los dos, aplicados por el sistema operativo; los secretos, siempre afuera.**

## Practicá
> [!question]- ¿Qué podría hacer un script si hereda todas las credenciales del servidor?
> Todo lo que puede hacer el servidor: leer y modificar datos de todos los clientes, llamar APIs con permisos de escritura, gastar con las claves de los proveedores de modelos y, si tiene red, enviar todo eso afuera. Una prompt injection en una página alcanza para ordenárselo. Dale solo la credencial mínima de su tarea, de corta duración y limitada a ese cliente, o dejá que un proxy fuera del sandbox haga las llamadas autenticadas sin exponer el secreto. Y limitá la red: sin salida, una credencial robada no se puede enviar.

> [!question]- Tu agente corre en un contenedor sin acceso al disco del host, pero con red abierta. ¿Está aislado?
> Solo a medias. No puede leer `~/.ssh` del host, pero todo lo que llegue al contenedor, como documentos del cliente, variables de entorno o resultados de tools, puede enviarlo a cualquier dominio si una inyección se lo ordena. Agregá un proxy de salida con lista de dominios y probá que una conexión a un dominio no permitido falle.

> [!question]- ¿Cómo probarías que el sandbox funciona sin depender de que el modelo se porte bien?
> Con pruebas que no pasan por el modelo: corré scripts dentro del sandbox que intenten leer `~/.ssh`, escribir fuera del directorio de trabajo, conectarse a un dominio no permitido y leer variables de entorno sensibles. Todas deben fallar por el sistema, y cada bloqueo debe quedar registrado. Después sumá casos con inyecciones reales en páginas o documentos a tus evals: [[Tool evals]] y [[Regresiones y CI]].

## Se conecta con
[[Guardrails]] · [[Control humano y permisos]] · [[Seguridad y evidencia documental]] · [[Tool evals]] · [[ACLs]]
