---
tipo: concepto
dominio: runtime
estado: por-ver
parent: "[[Runtime de agentes]]"
prereqs: ["[[Ejecución y recuperación]]"]
se_evalua_con: ["[[Evals de rendimiento]]"]
contrasta_con: []
fuentes:
  - https://sre.google/sre-book/monitoring-distributed-systems/
  - https://prometheus.io/docs/practices/histograms/
  - https://docs.vllm.ai/en/stable/usage/metrics/
bloque: "03 · Operación"
orden: 300
aliases: [Latencias, Latencia p95, p95]
---
# Latencia y percentiles

> **En una frase:** medí cuánto espera el usuario y cuánto tardan las respuestas lentas, además de la típica.

## Qué tiempo estás midiendo
| Medida | Desde → hasta |
|---|---|
| **Latencia total** | El usuario envía la petición → recibe el resultado completo; incluye cola, modelo, tools y reintentos |
| **TTFT** —tiempo al primer token— | Se envía la petición → llega el primer token de salida; indicá si medís en el cliente o en el servidor del modelo |
| **Por etapa** | Inicio → fin de retrieval, reranking, una llamada al modelo o una tool; se ve en [[Trazas y debugging]] |

En [[Streaming y cancelación]], registrá también cuándo el usuario ve el primer texto: un evento de conexión o de progreso no cuenta como respuesta. Un TTFT bajo puede convivir con una latencia total alta. Los servidores de inferencia también distinguen estas medidas, pero no incluyen necesariamente todo tu flujo. [Métricas de vLLM](https://docs.vllm.ai/en/stable/usage/metrics/).

## p50, p95 y p99
Ordená los tiempos de menor a mayor. Un percentil marca un punto de esa distribución:

| Percentil | Cómo leerlo |
|---|---|
| **p50** | Mediana: la mitad de las mediciones queda en ese tiempo o por debajo |
| **p95** | El 95 % queda en ese tiempo o por debajo; muestra la zona lenta |
| **p99** | El 99 % queda en ese tiempo o por debajo; mira más cerca del extremo |

**p95 = 5 s** significa que aproximadamente el 95 % de las mediciones tarda como máximo 5 s. No es el promedio ni el máximo, y el resto puede tardar mucho más. [Percentiles en Prometheus](https://prometheus.io/docs/practices/histograms/).

**Ejemplo ilustrativo:** 94 peticiones tardan 1 s, una tarda 5 s y cinco tardan 12 s. Con el método de rango más cercano, sobre 100 tiempos: p50 = 1 s, p95 = 5 s y p99 = 12 s; el promedio es 1,59 s. Mirar solo el promedio oculta esas esperas largas. [Latencia en Google SRE](https://sre.google/sre-book/monitoring-distributed-systems/).

## Cómo medir sin confundirse
- **Fijá el alcance:** cliente o servidor, inicio y fin, unidad, ventana, versión y cantidad de mediciones. Usá un reloj monotónico para duraciones dentro de un proceso.
- **Compará grupos equivalentes:** consultas simples y complejas, cliente, carga y caché fría/caliente. Pocas mediciones hacen inestable el p95; p99 requiere aún más datos.
- **Conservá los fallos:** reportá sus tiempos y su tasa aparte de las respuestas completas. Un timeout a los 10 s no es una respuesta completada en 10 s; las cancelaciones también se identifican.
- **No sumes percentiles ni ramas paralelas:** medí el total de punta a punta. Tampoco promedies p95 de servidores o ventanas: reuní las observaciones o histogramas compatibles y recalculá. [Agregación de percentiles](https://prometheus.io/docs/practices/histograms/).

## Practicá
> [!question]- Baja el TTFT, pero el p95 total sube. ¿Mejoró la experiencia?
> La respuesta empieza antes, pero las ejecuciones lentas terminan después. Mirá ambos tiempos, los errores y las trazas: puede haber más tokens de salida, tools lentas o espera en cola. Compará con los límites de [[Evals de rendimiento]].

## Se conecta con
[[Trazas y debugging]] → [[Operación en producción]] → [[Despliegue y operación bajo carga]]. Las pruebas están en [[Evals de rendimiento]].
