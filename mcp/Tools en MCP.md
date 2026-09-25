---
tipo: concepto
dominio: mcp
bloque: "01 · Componentes"
orden: 120
estado: por-ver
parent: "[[MCP]]"
prereqs: ["[[Arquitectura MCP]]"]
se_evalua_con: ["[[Tool evals]]"]
contrasta_con: ["[[Resources en MCP]]", "[[Prompts en MCP]]"]
fuentes: ["https://modelcontextprotocol.io/specification/2026-07-28/server/tools"]
---
# Tools en MCP

> **En una frase:** una tool ejecuta una operación acotada: buscar información, calcular o modificar un sistema.

## Contrato

El servidor publica tools mediante `tools/list`; el cliente ejecuta una mediante `tools/call`. Cada definición incluye nombre, descripción e `inputSchema`; puede incluir `outputSchema`. El resultado puede contener texto y `structuredContent`. Si se declara un esquema de salida, el resultado estructurado debe respetarlo. [Especificación de tools](https://modelcontextprotocol.io/specification/2026-07-28/server/tools).

**Diseño del ejemplo:**

| Parte | Decisión |
|---|---|
| Nombre | `buscar_documentos` |
| Descripción | Buscar fragmentos del manual para fundamentar respuestas |
| Entradas | `query`: texto no vacío; `top_k`: entero entre 1 y 5 |
| Salida | Lista de `document_id`, `uri`, `fragmento` y `version` |
| Efectos | Solo lectura; no cambia documentos |
| Vacío | `hits: []` significa que no se encontró evidencia |

## Quién decide usarla

Se describe como una capacidad **controlada por el modelo** porque normalmente este propone la llamada. El host puede impedirla o requerir aprobación; el servidor vuelve a autorizarla. Las anotaciones como `readOnlyHint` describen intención, pero no son barreras de seguridad.

## Errores que conviene distinguir

- **Transporte/autenticación:** no se pudo conectar o el token no sirve.
- **Protocolo:** mensaje inválido o método desconocido.
- **Ejecución:** la operación falla; puede devolverse un resultado con `isError: true`.
- **Sin resultados:** búsqueda válida sin coincidencias; no es necesariamente un error.

**Ejemplo de diseño propio:** separar `consultar_reembolso` de `aprobar_reembolso` permite darles permisos diferentes. Una tool enorme llamada `hacer_cualquier_cosa` dificulta validación y evaluación.

## Se conecta con

[[RAG como tool MCP]] · [[Construir un servidor MCP]] · [[Control humano y permisos]] · [[Tool evals]]
