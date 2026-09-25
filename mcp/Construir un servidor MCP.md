---
tipo: concepto
dominio: mcp
bloque: "03 · Implementación"
orden: 310
estado: por-ver
parent: "[[MCP]]"
prereqs: ["[[Tools en MCP]]", "[[Resources en MCP]]", "[[Prompts en MCP]]", "[[Conexión y transportes MCP]]"]
se_evalua_con: ["[[Tool evals]]"]
contrasta_con: []
fuentes: ["https://github.com/modelcontextprotocol/python-sdk/tree/v1.x"]
---
# Construir un servidor MCP

> **En una frase:** registrá contratos pequeños sobre tu lógica, elegí transporte y verificá el recorrido desde un cliente.

## Laboratorio reproducible

**Python 3.11+ y `uv`. SDK oficial `mcp==1.30.0`, protocolo `2025-11-25`.** Usa FastMCP incluido en `mcp`, no el paquete independiente `fastmcp`. El corpus es público y ficticio; la búsqueda por palabras ilustra recuperación sin embeddings ni servicios externos. [SDK Python oficial](https://github.com/modelcontextprotocol/python-sdk/tree/v1.x).

Guardá este bloque como `server.py` en una carpeta de práctica:

```python
import argparse
import re
from typing import Annotated

from mcp.server.fastmcp import FastMCP
from pydantic import Field

mcp = FastMCP("Manual de soporte", stateless_http=True, json_response=True)

DOCUMENTOS = {
    "reembolsos": "Para solicitar un reembolso, abrí un ticket con el número de pedido.",
    "envios": "Para consultar el envío, buscá el número de seguimiento en tu pedido.",
}


@mcp.tool()
def buscar_documentos(
    query: Annotated[str, Field(min_length=1, max_length=500)],
    top_k: Annotated[int, Field(ge=1, le=5)] = 3,
) -> dict[str, list[dict[str, str]]]:
    """Busca fragmentos del manual público. Devuelve evidencia y URI; no modifica datos."""
    palabras = set(re.findall(r"\w+", query.casefold()))
    if not palabras:
        raise ValueError("La consulta debe contener palabras")
    candidatos = []
    for doc_id, texto in DOCUMENTOS.items():
        tokens = set(re.findall(r"\w+", texto.casefold()))
        score = len(palabras & tokens)
        if score:
            candidatos.append((score, doc_id, texto))
    candidatos.sort(key=lambda item: (-item[0], item[1]))
    return {"hits": [
        {"document_id": doc_id, "uri": f"docs://manual/{doc_id}",
         "fragmento": texto, "version": "demo-v1"}
        for _, doc_id, texto in candidatos[:top_k]
    ]}


@mcp.resource("docs://manual/{document_id}", mime_type="text/plain")
def leer_documento(document_id: str) -> str:
    """Lee un documento público del manual de demostración."""
    if document_id not in DOCUMENTOS:
        raise ValueError("Documento no disponible")
    return DOCUMENTOS[document_id]


@mcp.prompt()
def responder_con_fuentes(pregunta: str) -> str:
    """Prepara una pregunta que debe responderse con evidencia del manual."""
    return (
        "Consultá el manual y citá las URI utilizadas. "
        "Si no hay evidencia suficiente, explicalo. "
        f"Pregunta del usuario: {pregunta}"
    )


if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument("--http", action="store_true")
    args = parser.parse_args()
    mcp.run(transport="streamable-http" if args.http else "stdio")
```

## Conectarlo

Para **stdio**, configurá en tu host el ejecutable absoluto de `uv` y estos argumentos, reemplazando la ruta de ejemplo por la real:

```json
["run", "--with", "mcp==1.30.0", "python", "/ruta/absoluta/server.py"]
```

El host lanza el proceso. Para **HTTP**, corré desde la carpeta del archivo:

```bash
uv run --with 'mcp==1.30.0' python server.py --http
```

Conectá un cliente compatible con `2025-11-25` a `http://127.0.0.1:8000/mcp`. Ese proceso escucha localmente y no tiene autenticación. La siguiente nota explica cómo protegerlo antes de usar fuentes privadas: [[Autenticación y autorización MCP]].

## Comprobarlo con un cliente o Inspector

| Operación | Resultado esperado |
|---|---|
| Inicializar y `tools/list` | Aparece `buscar_documentos` |
| Buscar `reembolso`, `top_k=1` | Fragmento y URI de reembolsos |
| Buscar `astronomía` | `hits: []` |
| `top_k=0` o consulta vacía | Error de validación |
| `resources/templates/list` | Plantilla `docs://manual/{document_id}` |
| Leer `docs://manual/reembolsos` | Texto del documento |
| Leer ID inexistente | Error sin contenido de otro documento |
| `prompts/get` para `responder_con_fuentes` | Mensaje con la pregunta |

Probá el contrato sin un modelo primero. Después verificá que el host elige la tool y usa sus resultados correctamente. El servidor solo publica una plantilla de resource; no esperes que cada documento aparezca en `resources/list`.

**Verificación del ejemplo:** los bloques de servidor y autenticación se ejecutaron con Python 3.13 y las dependencias fijadas. Se comprobaron llamadas por stdio, búsqueda vacía, argumentos inválidos, lectura por URI y obtención del prompt. El complemento JWT se comprobó por HTTP en memoria con claves locales: acceso válido, tokens inválidos, scope insuficiente y metadata OAuth. No se probó el login con un proveedor externo.

## Se conecta con

[[RAG como tool MCP]] · [[Autenticación y autorización MCP]] · [[Tool evals]]
