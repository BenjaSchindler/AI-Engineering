---
tipo: mapa
dominio: runtime
estado: por-ver
prereqs: ["[[Tools y function calling]]", "[[Contexto, memoria y estado]]"]
se_evalua_con: ["[[Tool evals]]", "[[Regresiones y CI]]"]
contrasta_con: []
fuentes: []
---
# Runtime de agentes

> **En una frase:** controlar la ejecución, reutilizar trabajo válido y operar el sistema con usuarios reales.

```mermaid
flowchart LR
  c[Preparar contexto] --> m[Llamar al modelo]
  m --> t[Validar y ejecutar tools]
  t --> s[Guardar estado]
  s --> c
  m --> f[Finalizar o detener]
```

## Tres bloques
| Bloque | Recorrido |
|---|---|
| **Ejecución** | [[Presupuesto de contexto]] → [[Ejecución y recuperación]]; después [[Streaming y cancelación]] y [[Resiliencia entre proveedores]] |
| **Caché** | [[Caché en agentes]] → claves e invalidación → elegir entre prompts, respuestas o ingesta/búsqueda |
| **Operación** | [[Trazas y debugging]] → [[Operación en producción]] → [[Despliegue y operación bajo carga]] |

Las capas de caché comparten reglas de validez, pero no forman un pipeline. Tienen su [[Caché.canvas|sub-mapa de Caché]] para no mezclar sus detalles con la ejecución.

## Conexiones con otras categorías
[[Seguridad]] define permisos y aislamiento. [[Evals]] mide calidad y regresiones: [[Regresiones y CI]] antes de publicar, [[Evals por cliente y producción]] durante el uso real. Operar pertenece aquí; evaluar pertenece a Evals.

## Practicá
> [!question]- ¿Cómo contarías una falla de punta a punta: cómo la encontraste en las trazas y cómo comprobaste la recuperación?
> Con cinco partes: síntoma, traza, causa, arreglo y prueba. Por ejemplo: un cliente recibió dos cotizaciones. En la traza había dos llamadas de creación en la misma ejecución, separadas por un timeout y sin clave de idempotencia; el runtime reintentaba a ciegas. El arreglo fue registrar la clave en el checkpoint antes de llamar y reintentar con la misma. La prueba: un replay que inyecta el timeout después de crear, un golden case que verifica una sola cotización en el estado final, y esa prueba sumada a CI. Ver [[Ejecución y recuperación]] y [[Trazas y debugging]].

## Estado de los nodos
```dataview
TABLE WITHOUT ID file.link AS nodo, bloque, estado
FROM "runtime"
WHERE file.name != "Runtime de agentes" AND (file.folder = "runtime" OR file.name = "Caché en agentes")
SORT orden
```

[[Runtime de agentes.canvas|Abrir el canvas de Runtime]] · [[Subagents y Deep Agents]]

Para integraciones distribuidas: [[MCP stateless y estado]] distingue estado del protocolo, conversación y persistencia de negocio.
