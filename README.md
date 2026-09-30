# AI Engineering · grafo de aprendizaje

Vault de Obsidian con **diez categorías principales**. Cada nota tiene una ubicación principal; los enlaces conectan sus aplicaciones en otras áreas. Los canvas ordenan las tarjetas por tema y recorrido de lectura.

## Progreso
Marcá tu avance con `estado` en el frontmatter de cada nota: `por-ver` → `aprendiendo` → `dominado`. Una nota `aprendiendo` cuenta como media en el avance; los mapas no cuentan.

```dataview
TABLE WITHOUT ID
  "<progress value='" + sum(rows.puntos) + "' max='" + length(rows) + "'></progress>" AS "Avance",
  round(100 * sum(rows.puntos) / length(rows)) + " %" AS "%",
  length(filter(rows.estado, (e) => e = "dominado")) + " / " + length(rows) AS "Dominadas",
  length(filter(rows.estado, (e) => e = "aprendiendo")) AS "Aprendiendo",
  length(filter(rows.estado, (e) => e = "por-ver")) AS "Por ver"
FROM -"templates"
WHERE estado AND tipo != "mapa"
FLATTEN choice(estado = "dominado", 1, choice(estado = "aprendiendo", 0.5, 0)) AS puntos
GROUP BY "vault"
```

### Por categoría
El siguiente es la primera nota `por-ver` en el orden de lectura de su categoría. Las notas de un sub-mapa, como Caché, toman la posición de su hub.

```dataview
TABLE WITHOUT ID
  object("ml-gen-ai", link("ML para Gen AI"), "fundamentos", link("Fundamentos"), "mcp", link("MCP"), "rag", link("RAG"), "multiagente", link("Multiagentes"), "runtime", link("Runtime de agentes"), "seguridad", link("Seguridad"), "evals", link("Evals"), "programacion-agentes", link("Programación con agentes"), "clouds", link("Clouds para Gen AI"))[key] AS "Categoría",
  "<progress value='" + sum(rows.puntos) + "' max='" + length(rows) + "'></progress>" AS "Avance",
  length(filter(rows.estado, (e) => e = "dominado")) + " / " + length(rows) AS "Dominadas",
  length(filter(rows.estado, (e) => e = "aprendiendo")) AS "Aprendiendo",
  filter(rows, (r) => r.estado = "por-ver")[0].file.link AS "Siguiente"
FROM -"templates"
WHERE estado AND tipo != "mapa"
FLATTEN choice(estado = "dominado", 1, choice(estado = "aprendiendo", 0.5, 0)) AS puntos
SORT default(parent.orden, orden) ASC, orden ASC, file.name ASC
GROUP BY dominio
SORT object("ml-gen-ai", 0, "fundamentos", 1, "mcp", 2, "rag", 3, "multiagente", 4, "runtime", 5, "seguridad", 6, "evals", 7, "programacion-agentes", 8, "clouds", 9)[key] ASC
```

### Estudiando ahora
```dataview
TABLE WITHOUT ID file.link AS "Nota", parent AS "Mapa", bloque AS "Bloque"
FROM -"templates"
WHERE estado = "aprendiendo" AND tipo != "mapa"
SORT dominio ASC, default(parent.orden, orden) ASC, orden ASC
```

> [!note]- Todas las notas por ver
> ```dataview
> TABLE WITHOUT ID file.link AS "Nota", parent AS "Mapa", bloque AS "Bloque"
> FROM -"templates"
> WHERE estado = "por-ver" AND tipo != "mapa"
> SORT dominio, default(parent.orden, orden), orden, file.name
> ```

## Cómo navegar
1. **[[Mapa.canvas|Mapa general]]:** elegir una categoría.
2. **Sub-mapa:** seguir los grupos numerados, de arriba hacia abajo y de izquierda a derecha.
3. **Nota:** leer el concepto, el ejemplo y sus conexiones. Las tablas Dataview siguen el mismo orden de estudio.

Las flechas llevan una etiqueta: **flujo** para datos, **lectura** para estudio y **requiere** para una base necesaria. Una línea **contrasta** compara alternativas; no indica que deban ejecutarse seguidas. Estar en el mismo grupo tampoco implica un pipeline.

## Categorías
| Categoría | Qué pertenece aquí | Sub-mapa |
|---|---|---|
| [[ML para Gen AI]] | Matemáticas, datos, entrenamiento y métricas de ML aplicadas a RAG y LLMs | [[ML para Gen AI.canvas]] |
| [[Fundamentos]] | Modelos, prompts, contexto, tools y elección entre prompting, RAG y fine-tuning | [[Fundamentos.canvas]] |
| [[MCP]] | Arquitectura, tools, resources, prompts, transportes, stateless, RAG y autenticación | [[MCP.canvas]] |
| [[RAG]] | Preparación de fuentes, índices, recuperación y respuestas con evidencia | [[RAG.canvas]] |
| [[Multiagentes]] | Elegir arquitectura, transferir control, delegar y coordinar | [[Multiagentes.canvas]] |
| [[Runtime de agentes]] | Ejecución, recuperación, caché, latencia y operación en producción | [[Runtime de agentes.canvas]] |
| [[Seguridad]] | Autorización, ACLs, guardrails, aislamiento y auditoría | [[Seguridad.canvas]] |
| [[Evals]] | Casos, evaluadores, calidad, rendimiento y regresiones | [[Evals.canvas]] |
| [[Programación con agentes]] | Preparar el repositorio, trabajar en cambios y revisarlos | [[Programación con agentes.canvas]] |
| [[Clouds para Gen AI]] | Comparar AWS, GCP y Azure: modelos, agentes, RAG y servicios básicos | [[Clouds para Gen AI.canvas]] |

## Recorridos
**Clouds para Gen AI:** [[Mapa.canvas|Mapa general]] enlaza a un [[Clouds para Gen AI.canvas|sub-mapa propio]] con un primer nodo de resumen e imagen. Dentro, tres columnas comparables con ocho filas: modelos → agentes → recuperación RAG → almacenamiento → contenedores → funciones → secretos → observabilidad. AWS, GCP y Azure tienen un color propio; los servicios de una misma función quedan a la misma altura.

**ML para Gen AI:** [[ML para Gen AI]] reúne bases matemáticas → datos y validación → entrenamiento → métricas → representaciones y LLMs → adaptación y uso real → caso práctico. Su canvas muestra resúmenes con imágenes; cada tarjeta enlaza a la nota completa.

**Para entender recall, precision y ROC:** [[Matriz de confusión y accuracy]] → [[Precision y recall]] → [[F1, F-beta y promedios por clase]] → [[Umbrales y costo de errores]] → [[Curvas ROC y AUC]] → [[Curva Precision-Recall y Average Precision]] → [[Calibración y confianza]]. Si estás empezando desde cero, seguí primero los bloques de bases y datos.

**Base común:** [[LLMs y elección de modelo]] → [[Prompts y salidas estructuradas]] → [[Contexto, memoria y estado]] → [[Tools y function calling]].

Después elegí según la tarea: [[MCP]] para conectar capacidades externas; [[RAG]] si necesitás fuentes; [[Multiagentes]] si necesitás estudiar coordinación; [[Runtime de agentes]] para ejecutar y operar. [[Seguridad]] y [[Evals]] acompañan todas esas decisiones. Programación con agentes puede estudiarse por separado.

**RAG:** panorama → ingesta → índice y recuperación → variantes. La ingesta prepara el índice; la consulta sigue [[Query rewrite]] → [[Bi-encoder]] + BM25 → [[Búsqueda híbrida y reranking|fusión]] → [[Cross-encoder]] → contexto → respuesta. Rewrite, híbrida y reranking son opcionales. HNSW e IVF son alternativas, no dos pasos del pipeline.

**MCP:** arquitectura → tools, resources y prompts → conexión y estado → servidor de ejemplo → RAG como tool → autenticación y autorización. Las notas distinguen versiones del protocolo y del SDK.

**Evals:** casos y criterios → elegir qué evaluar → coordinación, si aplica → decidir y monitorear → [[Caso práctico - Asistente de soporte]].

## Dónde quedó cada tema transversal
- **Bases de ML:** [[ML para Gen AI]] explica conceptos y métricas; [[Evals]] conserva las pruebas del sistema y [[RAG evals]] su aplicación a recuperación y respuestas.
- **Caché:** [[Caché en agentes]] es un subtema de Runtime con su [[Caché.canvas|propio sub-mapa]]. Empezá por [[Claves e invalidación de caché]]; después elegí [[Prompt caching]], [[Caché de respuestas]] o [[Caché de ingesta y búsqueda]]. Esta última conserva el alias `Caching`.
- **Permisos de documentos:** [[ACLs]] vive en Seguridad y sigue enlazada desde ingesta y recuperación.
- **Operación:** [[Latencia y percentiles]], [[Trazas y debugging]], [[Operación en producción]] y [[Despliegue y operación bajo carga]] viven en Runtime. [[Evals por cliente y producción]] permanece en Evals porque trata de evaluar resultados.
- **Pruebas especializadas:** [[Evals de rendimiento]], [[Evals de caché]], [[RAG evals]] y las pruebas de subagentes permanecen en Evals; cada área enlaza a sus pruebas.

## Carpetas
| Carpeta | Contenido |
|---|---|
| `ml-gen-ai/` | Hub y 28 notas de bases de ML para Gen AI |
| `fundamentos/` | Bases y decisiones de adaptación |
| `mcp/` | Protocolo, conexión, estado e implementación de integraciones |
| `rag/` y `rag/ingesta/` | Recuperación, variantes y preparación de datos |
| `multiagente/` y `multiagente/subagents/` | Arquitectura, patrones y delegación |
| `runtime/` | Ejecución y operación |
| `runtime/cache/` | Hub y cuatro fichas de caché |
| `seguridad/` | Permisos, contención y evidencia |
| `evals/` | Evaluación y caso práctico |
| `programacion-agentes/` | Prácticas de desarrollo |
| `clouds/` | Hub y 24 fichas comparables de AWS, GCP y Azure |
| `assets/clouds/` | Iconos oficiales de los servicios y sus fuentes |
| `canvas/` | Diez sub-mapas principales y el sub-mapa de Caché |

## Cómo mantener el orden
La plantilla está en `templates/Nodo.md`. **`dominio`** define la categoría; **`parent`** el hub; **`bloque`** agrupa el tema; **`orden`** fija su posición de lectura. `prereqs` declara conocimientos previos, `se_evalua_con` enlaza a las pruebas y `contrasta_con` a alternativas.

Al sumar una nota, elegí primero una categoría y un bloque existente. Creá otro subtema solo si reúne varias notas con una pregunta común. Actualizá el hub y su canvas; agregá enlaces desde otras áreas sin duplicar la nota.

Los diagramas Mermaid e ilustraciones SVG viven con las notas. El estado de cada nota alimenta el tablero de [[#Progreso]].
