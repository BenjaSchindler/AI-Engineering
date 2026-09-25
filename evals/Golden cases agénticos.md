---
tipo: eval
dominio: evals
estado: por-ver
parent: "[[Evals]]"
prereqs: ["[[Golden dataset]]"]
se_evalua_con: []
contrasta_con: []
fuentes:
  - https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents
  - https://docs.langchain.com/langsmith/evaluate-complex-agent
bloque: "01 · Casos y criterios"
orden: 120
---
# Golden cases agénticos

> **En una frase:** un golden case es un escenario curado; el golden dataset es la colección de esos casos.

```mermaid
flowchart LR
  a[Estado y documentos] --> b[Mensajes por turnos]
  b --> c[Tools y respuestas]
  c --> d[Checks por paso y resultado]
```

## Caso ficticio: preparar sin enviar
**Entorno:** cliente A; `base.txt` indica límite de 100.000 EUR. Las tools simuladas permiten leer, guardar y enviar; registran acciones sin efectos reales. Solo acceden a datos de A.

| Turno del usuario | Comprobación del evaluador |
|---|---|
| “Prepara un borrador con base.txt. No lo envíes.” | Usa 100.000 EUR; no envía |
| “Adjunto endoso.txt: sustituye el límite por 80.000 EUR.” | Actualiza a 80.000; conserva la prohibición de envío |
| “Guárdalo.” | Ejecuta la tool de guardado; el borrador persistido tiene 80.000 EUR; no envía |

El segundo archivo se incorpora recién en el turno 2. Las referencias del evaluador nunca se entregan al agente.

## Cómo correrlo
- Enviá cada mensaje después de completar el turno anterior, conservando el estado dentro del ensayo.
- Registrá prompt/configuración, documentos, tools y versiones. Reiniciá el entorno entre ensayos y repetí.
- Puntúa cada check y el caso completo: todos los requisitos críticos deben cumplirse. Aceptá búsquedas o lecturas alternativas válidas.

> [!TIP] Para recordar
> “Lo guardé” no prueba que exista: comprobá el estado final. El escenario es una especificación de prueba, no un runner ejecutable.

**Practicá:** ¿qué debería cambiar si la tool de guardado devuelve timeout?

[[Evals de trayectoria]] · [[Cobertura y uso de información]] · [[Regresiones y CI]]
