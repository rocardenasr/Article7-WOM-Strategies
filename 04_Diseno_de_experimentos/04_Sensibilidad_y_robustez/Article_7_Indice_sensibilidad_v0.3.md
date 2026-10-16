# Article 7 — Índice de la entrega de sensibilidad v0.3

La entrega contiene 16.800 simulaciones nuevas y la comparación con 700 ejecuciones basales. Empiece por el informe; el README del paquete Python explica la reproducción.

## Datos completos

Descargue las ocho partes y extráigalas en el mismo directorio vacío. Se combinan bajo `sensitivity_v0.3/`; sus rutas son disjuntas. La parte 01 contiene además los manifiestos globales, configuraciones, logs y resúmenes agregados. Los escenarios individuales contienen sus propias trazas.

| Archivo | Tamaño (MB decimales) | Escenarios |
|---|---:|---|
| Article_7_Sensibilidad_datos_v0.3_parte01.zip | 32.82 | conversion_low, conversion_high, need_low |
| Article_7_Sensibilidad_datos_v0.3_parte02.zip | 33.82 | need_high, campaign_cost_low, campaign_cost_high |
| Article_7_Sensibilidad_datos_v0.3_parte03.zip | 31.96 | reward_zero, reward_low, reward_high |
| Article_7_Sensibilidad_datos_v0.3_parte04.zip | 32.80 | budget_low, budget_high, capacity_stress |
| Article_7_Sensibilidad_datos_v0.3_parte05.zip | 32.72 | capacity_low, capacity_high, segregation_high |
| Article_7_Sensibilidad_datos_v0.3_parte06.zip | 33.97 | segregation_low, acquisition_threshold_low, acquisition_threshold_high |
| Article_7_Sensibilidad_datos_v0.3_parte07.zip | 33.51 | activity_threshold_low, activity_threshold_high, evidence_threshold_low |
| Article_7_Sensibilidad_datos_v0.3_parte08.zip | 35.77 | evidence_threshold_high, high_demand_conversion, high_demand_capacity_stress |

Los hashes SHA-256 y tamaños exactos están en `Article_7_Indice_datos_sensibilidad_v0.3.json`. Los datos basales mínimos para el análisis se incluyen en `reference_baseline/` dentro del paquete de código. Las trazas basales completas permanecen en la entrega v0.2.

## Otros archivos

| Archivo | Contenido |
|---|---|
| Article_7_Informe_sensibilidad_v0.3.md | Hallazgos, magnitudes, incertidumbre y límites |
| Article_7_Protocolo_sensibilidad_v0.3.md | Diseño fijado antes de ejecutar esta sensibilidad |
| Article_7_Verificacion_sensibilidad_v0.3.md | Evidencia y alcance de las comprobaciones |
| Article_7_Sensibilidad_Python_v0.3.zip | Código, pruebas, configuraciones y referencia basal |
| Article_7_Sensibilidad_Colab_v0.3.ipynb | Notebook de reproducción; ejecutar todo es opcional |
| Article_7_Sensibilidad_analisis_v0.3.zip | Tablas, contrastes, diagnósticos, manifiestos y resúmenes por ejecución |
| Article_7_Sensibilidad_figuras_v0.3.zip | Dos figuras científicas en PNG y SVG |

La clave global es `(scenario, run_id)`. La unidad independiente del análisis es la semilla; se usan 100 semillas emparejadas por contraste. Las 52.500 revaloraciones contables del tiempo no son simulaciones adicionales.
