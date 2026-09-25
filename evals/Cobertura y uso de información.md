---
tipo: eval
dominio: evals
estado: por-ver
parent: "[[Evals]]"
prereqs: ["[[Golden cases agénticos]]", "[[Graders]]"]
se_evalua_con: []
contrasta_con: ["[[RAG evals]]"]
fuentes:
  - https://docs.cloud.google.com/document-ai/docs/evaluate
bloque: "02 · Qué evaluar"
orden: 250
---
# Cobertura y uso de información

> **En una frase:** medí si la respuesta aplica los hechos y restricciones relevantes, aunque lleguen por mensajes, archivos completos o grep.

```mermaid
flowchart LR
  a[Fuentes originales] --> b[Contenido entregado al modelo]
  b --> c[Hallazgos detectados]
  c --> d[Información aplicada al resultado]
```

## Qué medir
Curá hechos y restricciones esperados, con fuente, versión, página o pasaje y criticidad.

| Métrica | Cálculo |
|---|---|
| Recall de hallazgos | Esperados correctamente reflejados / esperados |
| Precisión | Hallazgos emitidos correctos / emitidos |
| Respaldo | Afirmaciones respaldadas por sus fuentes / verificadas |
| Uso de restricciones | Restricciones aplicables cumplidas / aplicables |
| Omisión crítica | Casos con alguna omisión crítica / casos con hallazgos críticos |

Compará hechos equivalentes, sin contar repeticiones. Denominador cero: “no aplica”; no equivale a éxito.

**Ejemplo:** el usuario pide usar EUR y un endoso cambia el límite. Citar ambos documentos pero responder con USD y el límite antiguo falla el uso de información.

> [!TIP] Para recordar
> Páginas entregadas / páginas requeridas mide cobertura técnica, no comprensión. Comprobá también OCR, tablas y truncamiento. Un grep sin coincidencias no demuestra ausencia.

Código verifica campos y reglas explícitas; un juez calibrado compara significado con la referencia y evidencia. Ninguno descubre automáticamente todas las omisiones de una referencia incompleta.

## Practicá
> [!question]- La respuesta usa el límite antiguo aunque el endoso estaba entre los documentos. ¿Cómo sabés si el dato nunca llegó, fue ignorado o se perdió al resumir?
> Seguí el endoso por las etapas del diagrama con la traza. **Nunca llegó:** no está en la entrada real del modelo porque la tool lo truncó, el OCR falló o el grep no lo encontró. **Se perdió al resumir:** estaba en una entrada anterior, pero no en el resumen que tenía el modelo al responder. **Fue ignorado:** estaba literal en la entrada final y la respuesta igual usó el límite antiguo; ahí probá posición y longitud con [[Lost in the middle]]. Cada causa tiene otro arreglo, por eso no alcanza con medir el resultado final.

## Se conecta con
[[Lost in the middle]] · [[Evals de trayectoria]] · [[Seguridad y evidencia documental]]
