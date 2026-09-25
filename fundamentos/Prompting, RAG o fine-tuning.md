---
tipo: concepto
dominio: fundamentos
estado: por-ver
parent: "[[Fundamentos]]"
prereqs: ["[[Prompts y salidas estructuradas]]"]
se_evalua_con: ["[[Golden dataset]]", "[[Regresiones y CI]]"]
contrasta_con: []
fuentes:
  - https://cloud.google.com/blog/products/ai-machine-learning/three-step-design-pattern-for-specializing-llms/
bloque: "02 · Elegir una mejora"
orden: 210
---
# Prompting, RAG o fine-tuning

> **En una frase:** elegí según la falla: instrucciones poco claras, información ausente o comportamiento que necesita entrenamiento.

| Opción | Qué cambia | Cuándo probarla |
|---|---|---|
| **Prompting** | Instrucciones y ejemplos del contexto | La tarea necesita criterios más claros; empezá por aquí |
| **RAG** | Evidencia recuperada en cada consulta | Necesitás fuentes propias, actualizables o citables |
| **Fine-tuning** | Parámetros del modelo o adaptadores mediante entrenamiento | Buscás un comportamiento especializado y tenés ejemplos de calidad para enseñarlo |

Pueden combinarse: un modelo ajustado también puede consultar documentos mediante RAG. No son tres escalones obligatorios. [Comparación de enfoques](https://cloud.google.com/blog/products/ai-machine-learning/three-step-design-pattern-for-specializing-llms/).

**Ejemplo:** para soporte, el prompt define el tono; RAG aporta el manual vigente; fine-tuning puede ayudar a clasificar tickets si la mejora justifica entrenar y mantener otra versión.

## Antes de entrenar
- Compará contra un baseline de prompt y ejemplos con el mismo [[Golden dataset]].
- Separá entrenamiento, validación y prueba; evitá duplicados que filtren respuestas entre conjuntos.
- Medí calidad, costo por tarea y regresiones. Entrenar no garantiza mejorar.

> [!TIP] Para recordar
> Fine-tuning no reemplaza un mecanismo para consultar datos que cambian ni asegura citas correctas. Para precios actuales, consultá la fuente; para ejecutar una acción, usá [[Tools y function calling]].

**Practicá:** ¿qué cambiarías primero si el modelo escribe bien, pero responde con una política antigua?

[[LLMs y elección de modelo]] · [[RAG básico]]
