---
tipo: concepto
dominio: mcp
bloque: "01 · Componentes"
orden: 130
estado: por-ver
parent: "[[MCP]]"
prereqs: ["[[Arquitectura MCP]]"]
se_evalua_con: ["[[RAG evals]]"]
contrasta_con: ["[[Tools en MCP]]"]
fuentes: ["https://modelcontextprotocol.io/specification/2026-07-28/server/resources"]
---
# Resources en MCP

> **En una frase:** un resource ofrece contenido que la aplicación puede leer e incorporar al contexto, identificado por una URI.

## Cómo se usa

| Método | Para qué sirve |
|---|---|
| `resources/list` | Descubrir recursos concretos publicados |
| `resources/templates/list` | Descubrir plantillas de URI parametrizadas |
| `resources/read` | Leer el recurso indicado por su URI |

`docs://manual/reembolsos` puede devolver texto y MIME type. `docs://manual/{document_id}` describe una familia de recursos. La URI identifica contenido dentro del servidor; no tiene que ser una URL navegable. Los resources son **controlados por la aplicación**: el host decide cómo ofrecerlos y cuándo agregarlos al contexto. [Especificación de resources](https://modelcontextprotocol.io/specification/2026-07-28/server/resources).

## Ejemplo

La tool de búsqueda devuelve tres fragmentos y sus URI. El host necesita ampliar uno y solicita `resources/read` sobre `docs://manual/reembolsos`. El servidor verifica permisos nuevamente y entrega el documento.

La búsqueda también podría devolver todo el fragmento necesario y evitar esa lectura extra. Elegí según tamaño, latencia y soporte del host; algunos hosts no exponen resources directamente al modelo.

## Qué no garantiza una URI

Conocer una dirección no da permiso para leerla. Tampoco convierte su contenido en instrucciones confiables. Un documento puede contener información maliciosa o desactualizada.

**Trade-off:** leer documentos completos aporta contexto, pero consume tokens. Para manuales grandes, empezá por fragmentos y ampliá solo cuando haga falta. Asociá versiones a las respuestas para detectar caché vieja.

## Se conecta con

[[RAG como tool MCP]] · [[ACLs]] · [[Presupuesto de contexto]] · [[Caché de ingesta y búsqueda]]
