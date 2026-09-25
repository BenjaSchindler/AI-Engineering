---
tipo: concepto
dominio: mcp
bloque: "01 · Componentes"
orden: 110
estado: por-ver
parent: "[[MCP]]"
prereqs: ["[[Tools y function calling]]"]
se_evalua_con: ["[[Tool evals]]"]
contrasta_con: []
fuentes: ["https://modelcontextprotocol.io/docs/2026-07-28/learn/architecture"]
---
# Arquitectura MCP

> **En una frase:** el host coordina la experiencia y el modelo; su cliente MCP habla con un servidor que expone capacidades.

## Quién hace qué

| Componente | Responsabilidad | En un asistente de soporte |
|---|---|---|
| Host | Conversación, contexto, modelo y políticas de ejecución | Aplicación donde preguntás |
| Cliente MCP | Intercambio de mensajes con un servidor | Adaptador MCP dentro del host |
| Servidor MCP | Publicar contratos y resolver operaciones autorizadas | Servicio conectado al manual |
| Backend | Datos y lógica de negocio | Buscador, documentos y CRM |

Un host puede conectar varios servidores mediante distintos clientes. El protocolo usa mensajes JSON-RPC; el SDK resuelve buena parte del intercambio. [Arquitectura oficial](https://modelcontextprotocol.io/docs/2026-07-28/learn/architecture).

## Ejemplo de punta a punta

1. El host descubre que existe `buscar_documentos` y presenta su descripción al modelo.
2. Ante “¿Cómo pido un reembolso?”, el modelo propone una llamada.
3. El host aplica sus políticas y el cliente envía `tools/call`.
4. El servidor valida argumentos y permisos, consulta el backend y devuelve fragmentos.
5. El host incorpora el resultado; el modelo redacta con evidencia.

El servidor de búsqueda no recibe necesariamente toda la conversación. Tampoco controla por sí solo la decisión final del modelo.

## Cuándo conviene

Usalo si querés reutilizar la misma capacidad desde varios hosts compatibles o separar la integración del agente. Para una única función interna, una llamada directa puede requerir menos infraestructura.

**Costo de la separación:** mantener contratos, compatibilidad, credenciales, errores y observabilidad entre procesos. MCP no aporta automáticamente planificación, memoria ni calidad de recuperación.

## Se conecta con

[[MCP]] · [[Conexión y transportes MCP]] · [[Agente individual vs workflow vs multiagente]]
