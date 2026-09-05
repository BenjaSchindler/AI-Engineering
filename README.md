# AI Engineering · grafo de aprendizaje

Vault de Obsidian: cada archivo es un nodo, cada `[[wikilink]]` es una arista. Todo es markdown + Mermaid, así que se lee igual en Obsidian, en GitHub y en cualquier editor.

![Mapa del grafo](assets/mapa.svg)

## Cómo navegar
Tres niveles, de lo general a lo específico:

1. **`Mapa.canvas`** → tres tarjetas (RAG, Multiagente, Evals) con sus nodos como links. Clic en un nombre abre la nota; *Abrir sub-mapa* abre el canvas del tema.
2. **`canvas/<tema>.canvas`** → cada tarjeta muestra la nota desde su título: nombre, frase y diagrama. Doble clic abre la nota completa.
3. **Nota hub** (`RAG`, `Multiagente`, `Evals`) → mindmap del tema, orden sugerido y tabla de estado (Dataview). Clic derecho → *Open local graph* muestra solo ese subgrafo; subiendo la profundidad se despliega el resto.

La vista de grafo global (Ctrl+G) permite explorar las conexiones entre notas. Podés personalizar sus filtros y grupos de colores desde Obsidian. Plugins incluidos en el vault: **Dataview** y **Excalidraw**.

## Estructura
| Carpeta | Dominio | Nodos |
|---|---|---|
| `rag/` | Hub **RAG**: de los datos a la respuesta | Vector DB (pipeline de ingesta), ANN HNSW, RAG básico, RAG agéntico |
| `rag/ingesta/` | Las 7 preguntas del pipeline, cuelgan de Vector DB | Connector, Normalization, Incremental sync, Document IDs, Deduplication, Caching, ACLs |
| `multiagente/` | Hub **Multiagente**: coordinación entre agentes | Orquestador workers, Router handoff |
| `evals/` | Hub **Evals**: cómo saber si funciona | Golden dataset, Tool evals, Multi-agent evals |
| `canvas/` | Un sub-canvas por hub | RAG, Multiagente, Evals |

## Anatomía de un nodo
Frontmatter con `tipo` (`mapa` para los hubs), `dominio`, `estado` (`por-ver` → `aprendiendo` → `dominado`), `parent` (el hub del que cuelga), `prereqs`, `se_evalua_con`, `contrasta_con`, `fuentes`. Después: una frase, un diagrama Mermaid, una tabla de cuándo sí / cuándo no, trade-offs. Plantilla en `templates/Nodo.md`.

## Tipos de arista
- `parent` → jerarquía: nodo → hub. Es lo que agrupa el grafo.
- `prereqs` → orden de estudio.
- `se_evalua_con` → conecta cada nodo con Evals.
- `contrasta_con` → pares que conviene entender juntos (RAG vs RAG agéntico, orquestador vs router).
- Wikilinks en el cuerpo → "se relaciona con".

## Qué estudiar ahora (Dataview)
```dataview
TABLE WITHOUT ID file.link AS nodo, parent, estado FROM "" WHERE estado = "por-ver" AND tipo != "mapa" SORT parent, file.name
```
