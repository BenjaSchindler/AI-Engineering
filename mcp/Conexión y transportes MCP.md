---
tipo: concepto
dominio: mcp
bloque: "02 · Conexión y estado"
orden: 210
estado: por-ver
parent: "[[MCP]]"
prereqs: ["[[Arquitectura MCP]]"]
se_evalua_con: ["[[Tool evals]]"]
contrasta_con: []
fuentes:
  - https://modelcontextprotocol.io/specification/2026-07-28/basic/transports/streamable-http
  - https://modelcontextprotocol.io/specification/2025-11-25/basic/lifecycle
  - https://modelcontextprotocol.io/specification/2025-11-25/basic/transports
---
# Conexión y transportes MCP

> **En una frase:** elegís cómo viajan los mensajes, verificás compatibilidad y descubrís qué ofrece el servidor.

## Dos transportes

| | stdio | Streamable HTTP |
|---|---|---|
| Arranque | El host lanza un proceso hijo | El servicio corre por separado |
| Configuración típica | Ejecutable, argumentos y entorno | URL del endpoint y autenticación |
| Mensajes | stdin/stdout | POST al endpoint MCP |
| Caso común | Integración local | Servicio compartido o remoto |
| Credenciales | Entorno/almacén de secretos del proceso | Token de acceso en cada petición protegida |

En stdio, `stdout` queda reservado para mensajes del protocolo; los logs van a `stderr`. El formato de configuración del host no está estandarizado: una entrada llamada `mcpServers` es una convención de algunos productos. [Transportes 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25/basic/transports).

## Revisiones: no mezclar los recorridos

| Paso | `2026-07-28` | `2025-11-25`, usado en el laboratorio |
|---|---|---|
| Descubrir versión y capacidades | `server/discover`, consulta opcional para el cliente | `initialize` obligatorio |
| Preparar operaciones | Metadatos por petición | `notifications/initialized` |
| Descubrir y usar | `tools/list`, `resources/read`, etc. | Los métodos disponibles según capacidades |
| Sesión HTTP del protocolo | Eliminada | Opcional; si el servidor la crea, usar `Mcp-Session-Id` |

La revisión actual declara versión y capacidades relevantes en `_meta` por petición. En HTTP también exige `MCP-Protocol-Version`, `Mcp-Method` y, para llamadas o lecturas concretas, `Mcp-Name`. El SDK debe soportar esa revisión; cambiar solo el número no migra un cliente. [Versionado](https://modelcontextprotocol.io/docs/2026-07-28/learn/versioning), [ciclo anterior](https://modelcontextprotocol.io/specification/2025-11-25/basic/lifecycle).

## HTTP y streaming

En `2026-07-28`, el endpoint acepta POST y puede responder JSON o SSE asociado a la petición. Las suscripciones usan `subscriptions/listen`; desapareció el stream GET anterior. SSE es un formato de streaming, no prueba de que exista una sesión. El antiguo transporte HTTP+SSE y Streamable HTTP son distintos. [Streamable HTTP actual](https://modelcontextprotocol.io/specification/2026-07-28/basic/transports/streamable-http).

## Diagnóstico rápido

1. **No arranca:** revisar ruta absoluta, ejecutable y dependencias.
2. **Conecta pero falla el protocolo:** comparar revisiones y transportes.
3. **No aparecen capacidades:** revisar anuncio del servidor y soporte del host.
4. **401/403:** revisar autenticación y permisos, no el prompt.
5. **Se corta:** revisar timeout, proxy y cancelación.

## Se conecta con

[[MCP stateless y estado]] · [[Construir un servidor MCP]] · [[Autenticación y autorización MCP]] · [[Streaming y cancelación]]
