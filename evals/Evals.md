---
tipo: mapa
dominio: evals
estado: por-ver
prereqs: ["[[Golden dataset]]"]
fuentes: []
---
# Evals

> **En una frase:** definir qué cuenta como éxito, medirlo y convertir las fallas en nuevas pruebas.

```mermaid
flowchart LR
  d[Casos y criterios] --> e[Ejecutar]
  e --> g[Puntuar calidad y medir tiempos y costo]
  g --> c[Comparar versiones]
  c --> f[Investigar fallas]
  f --> d
```

## Orden de lectura
| Bloque | Notas |
|---|---|
| **1. Casos y criterios** | [[Golden dataset]], [[Golden cases agénticos]], [[Graders]], [[Calibración de evaluadores]] |
| **2. Qué evaluar** | [[RAG evals]], [[Tool evals]], [[Evals de caché]], [[Evals de trayectoria]], [[Cobertura y uso de información]], [[Lost in the middle]], [[Evals de rendimiento]] |
| **3. Coordinación** | [[Multi-agent evals]] → [[Evaluación de subagentes]] → [[Decisión de delegar]] → [[Métricas de subagentes]] → [[Casos de eval de subagentes]] → [[Evals de Deep Agents]] |
| **4. Decidir y monitorear** | [[Regresiones y CI]] → [[Evals por cliente y producción]] |
| **5. Practicar** | [[Caso práctico - Asistente de soporte]] |

Las evals de RAG, tools, caché y rendimiento son pruebas complementarias; no hace falta ejecutar una para entender la siguiente. Para puntuar calidad, usá [[Graders|evaluadores calibrados]]; los tiempos se miden directamente. La coordinación es una especialización para sistemas que delegan.

## Bases de ML para interpretar resultados
Empezá por [[Matriz de confusión y accuracy]] y [[Precision y recall]]. Después: [[F1, F-beta y promedios por clase]], [[Curvas ROC y AUC]], [[Curva Precision-Recall y Average Precision]] y [[Calibración y confianza]]. [[Train, validation, test y leakage]] y [[Experimentos e incertidumbre]] ayudan a comparar versiones sin contaminar las pruebas. El recorrido completo está en [[ML para Gen AI]].

## Qué mide cada capa
- **Recuperación:** recall, precisión y ranking de evidencia.
- **Respuesta:** corrección, relevancia y respaldo en fuentes.
- **Acciones y coordinación:** tools, argumentos, permisos y estado final.
- **Sistema:** éxito de tarea, costo por tarea resuelta, latencia p50/p95 y errores bajo carga, también por cliente: [[Evals de rendimiento]].

## De las pruebas al uso real
[[Operación en producción]] y [[Despliegue y operación bajo carga]] viven en Runtime. Sus incidentes aportan casos al [[Golden dataset]]; las mediciones de calidad permanecen en esta categoría.

## Estado de los nodos
```dataview
TABLE WITHOUT ID file.link AS nodo, bloque, estado
FROM "evals" WHERE tipo != "mapa" SORT orden
```

[[Evals.canvas|Abrir el canvas de Evals]]
