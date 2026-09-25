---
tipo: eval
dominio: evals
estado: por-ver
parent: "[[Evals]]"
prereqs: ["[[Caché en agentes]]", "[[Golden cases agénticos]]"]
se_evalua_con: []
contrasta_con: []
fuentes:
  - https://redis.io/docs/latest/develop/use-cases/semantic-cache/
  - https://learn.microsoft.com/es-es/azure/architecture/patterns/cache-aside
bloque: "02 · Qué evaluar"
orden: 230
---
# Evals de caché

> **En una frase:** medí cuánto ahorra y cuántas veces reutiliza algo que ya no corresponde.

```mermaid
flowchart LR
  a[Mismos casos] --> b[Sin caché]
  a --> c[Con caché fría y caliente]
  b --> d[Comparar calidad, costo y latencia]
  c --> d
```

## Métricas de respuestas y tools
| Métrica | Cálculo |
|---|---|
| Hit rate | Hits / consultas de caché |
| Reutilización incorrecta | Hits cuya reutilización es inválida / hits evaluados |
| Respuestas obsoletas | Respuestas servidas con datos vencidos / respuestas evaluadas |
| Ahorro neto | Costo sin caché − costo con caché, incluyendo infraestructura y validación |

Reportá además éxito de tarea, latencia p95 y violaciones de permisos. Denominador cero → N/A. Para [[Prompt caching]], medí tokens de entrada cacheados / tokens de entrada y facturación real.

## Golden cases mínimos
- Misma pregunta, mismo contexto: reutilización válida.
- Nuevo endoso o mensaje correctivo: descartar respuesta anterior.
- Otro cliente o permiso revocado: no entregar contenido inaccesible.
- Preguntas similares con negación, moneda o fecha distinta: rechazar falso hit semántico.
- TTL vencido o caché caída: recalcular sin perder corrección.

**Cómo juzgar:** código para versiones, permisos, valores y expiración; referencia curada o juez calibrado para equivalencia semántica. No siempre hace falta un LLM.

**Para medir cambios del agente**, desactivá el caché de respuestas o aislalo por versión: devolver salidas viejas escondería regresiones. Para evaluar el producto, probá también caché activada, tráfico representativo y concurrencia.

**Practicá:** ¿por qué subir el hit rate puede empeorar el sistema?
