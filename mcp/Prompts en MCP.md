---
tipo: concepto
dominio: mcp
bloque: "01 · Componentes"
orden: 140
estado: por-ver
parent: "[[MCP]]"
prereqs: ["[[Arquitectura MCP]]"]
se_evalua_con: ["[[Tool evals]]"]
contrasta_con: ["[[Tools en MCP]]", "[[Resources en MCP]]"]
fuentes: ["https://modelcontextprotocol.io/specification/2025-11-25/server/prompts"]
---
# Prompts en MCP

> **En una frase:** un prompt MCP es una plantilla de mensajes reutilizable que el servidor prepara a partir de argumentos.

## Cómo se usa

`prompts/list` publica las plantillas y sus argumentos; `prompts/get` obtiene los mensajes de una plantilla concreta. Se consideran **controlados por el usuario**: por ejemplo, elegir “Responder con fuentes” en un menú. La interfaz exacta depende del host. [Contrato de prompts, revisión 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25/server/prompts).

## Ejemplo

**Nombre:** `responder_con_fuentes`.

**Argumento:** `pregunta = "¿Cómo solicito un reembolso?"`.

**Mensaje preparado:** “Consultá el manual para responder esta pregunta. Citá las URI utilizadas y explicá si falta evidencia. Pregunta: ¿Cómo solicito un reembolso?”.

Obtener esa plantilla no ejecuta automáticamente la búsqueda ni llama a un LLM. El host utiliza los mensajes y continúa su flujo.

## Diferencias útiles

| Concepto | Función |
|---|---|
| Prompt MCP | Plantilla que se descubre y obtiene por protocolo |
| System prompt del host | Instrucciones de la aplicación al modelo |
| Tool | Operación que puede producir un efecto o un resultado |
| Resource | Contenido que puede aportar contexto |

Un prompt compartido ayuda a repetir una tarea, pero no concede permisos ni asegura que el modelo siga cada instrucción. Versioná las plantillas y evaluá si mejoran respuestas reales.

## Se conecta con

[[Prompts y salidas estructuradas]] · [[Construir un servidor MCP]] · [[Golden dataset]]
