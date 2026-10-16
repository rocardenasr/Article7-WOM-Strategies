# Article 7 — Informe de sensibilidad v0.3

**Ejecución:** 26 de septiembre de 2026, UTC. **Motor:** Python v0.2.0, sin cambios. **Diseño y análisis:** v0.3.

## Resultado principal

Se completaron **16.800 simulaciones nuevas**: 24 escenarios × siete políticas × 100 semillas. Al incorporar las 700 ejecuciones basales existentes, el análisis compara 17.500 combinaciones escenario–política–semilla. Las 29 pruebas de software y las auditorías de todas las ejecuciones finalizaron sin fallos.

En el rango estudiado, el costo del tiempo y la prioridad de las reglas adaptativas son determinantes para interpretar los resultados. La política mixta suele alcanzar más clientes, pero eso no implica mayor utilidad. Con tiempo valorado en 2 UM/h, WOM orgánico presenta la mayor utilidad ajustada media en las 25 condiciones, incluido un empate con incentivos cuando la recompensa es cero. Esta regularidad describe el modelo y los parámetros ensayados; no establece superioridad universal ni constituye una recomendación comercial.

## Qué se comparó y cómo

Las políticas son A: WOM orgánico; B: referidos; C: incentivos; D: eventos; E: mezcla fija; F: secuencia fija; G: adaptación por reglas. Cada trayectoria dura 60 meses y parte de una población de 600 personas, incluidos 15 clientes iniciales. Los resultados de clientes son **nuevos clientes acumulados**, sin sumar esos 15 iniciales. La referencia tiene presupuesto de 1.200 UM, capacidad de 20 ventas mensuales y valoración del tiempo de 2 UM/h.

Se alteraron conversión, necesidad, costos de referidos/eventos, recompensa, presupuesto, capacidad, segregación de la red y umbrales de G. Dos escenarios combinaron necesidad y conversión altas, con y sin capacidad restrictiva. Los valores completos constan en el protocolo y en `configs/sensitivity_design_v0.3.json`. El bloque de costos cambia conjuntamente referidos y eventos; el bloque de adquisición cambia conjuntamente dos umbrales. Por tanto, sus componentes no se identifican por separado.

El diseño se fijó después de observar el basal y antes de ejecutar esta sensibilidad. Es una sensibilidad por bloques con dos escenarios conjuntos, no un análisis global ni un factorial completo. Las semillas 2001–2100 están emparejadas entre políticas y escenarios. **Cada contraste dispone de 100 réplicas independientes por semilla**; los 17.500 casos no son réplicas independientes del mismo contraste.

Las diferencias se calcularon dentro de cada semilla y los intervalos se obtuvieron con 5.000 remuestreos por bloques de semillas, usando los mismos índices en todos los contrastes. Los IC95% son intervalos percentiles marginales de incertidumbre Monte Carlo; no representan incertidumbre empírica ni cobertura simultánea de todos los contrastes. Las tablas completas incluyen error estándar Monte Carlo. No se reportan p-valores.

## 1. El resultado económico depende de valorar el tiempo

En el basal, C produce una utilidad operativa media de 48,00 UM frente a 30,40 UM de A. Al descontar el tiempo a 2 UM/h, C pasa a −38,74 UM, mientras A permanece en 30,40 UM. La igualdad entre sus **utilidades medias** ocurre aproximadamente en **0,406 UM/h**, con IC95% [0,333; 0,477]. Se obtiene como diferencia media de utilidad operativa dividida por diferencia media de horas: 17,60 / 43,369. Es un umbral condicionado a los supuestos, no un costo de oportunidad estimado empíricamente ni un óptimo individual.

C tiene la mayor utilidad operativa media en las 24 condiciones con recompensa positiva; en la condición de recompensa cero, C y A empatan exactamente. A tiene la mayor utilidad ajustada media con w=2 en las 25 condiciones, incluido ese empate. El orden de las medias no prueba separación estadística entre todas las alternativas.

Las valoraciones 0, 2 y 4 UM/h se aplicaron sobre las mismas trayectorias, porque este parámetro solo afecta la contabilidad del modelo. Son **52.500 evaluaciones contables**, no nuevas simulaciones. La neutralidad conductual se comprobó mediante ejecuciones directas con valores alternativos.

## 2. La prioridad de adquisición limita el uso de incentivos en G

En los 6.000 meses de ejecución de G en el basal, 563 cumplían las condiciones numéricas de activación de incentivos, pero 555 se resolvían antes mediante la regla de adquisición por referidos. Solo ocho meses, distribuidos en ocho ejecuciones, terminaron usando incentivos. Este diagnóstico se refiere a condiciones numéricas; no afirma que todos esos meses hubieran superado además cada restricción de factibilidad de una campaña de incentivos.

Elevar el umbral de actividad de 0,20 a 0,24 genera 912 meses que cumplen esas condiciones, pero 903 quedan precedidos por adquisición y solo nueve usan incentivos. Variar únicamente el umbral de actividad tiene poco efecto sobre el resultado final porque la prioridad de decisión continúa dominando.

Reducir conjuntamente los umbrales de adquisición de 30 clientes / 10 promotores a 24 / 8 reduce los clientes nuevos de G de 17,41 a 13,62: cambio de **-3,79; IC95% [-4,34; -3,21]**. A la vez, mejora la utilidad ajustada de −885,53 a −513,55 UM: **371,97; IC95% [320,96; 422,71] UM**. Aumentarlos a 36 / 12 eleva la captación en 2,04; IC95% [1,62; 2,46] clientes y produce un cambio en utilidad ajustada de -272,62; IC95% [-315,40; -229,98] UM.

El resultado evidencia un intercambio entre adquisición y recursos consumidos. No muestra que el umbral bajo sea óptimo ni aísla el efecto individual de cada uno de los dos umbrales.

## 3. Reducir costos puede prolongar la actividad y aumentar el costo del tiempo

Al reducir a la mitad los costos monetarios de referidos y eventos, E aumenta sus clientes nuevos de 26,79 a 34,93: **8,14; IC95% [7,22; 9,07]**. Su utilidad operativa mejora de −620,05 a −446,64 UM, pero su utilidad ajustada empeora de −1.329,43 a −1.507,07 UM: **-177,65; IC95% [-183,33; -171,87] UM**.

Las trazas muestran que E duplica el número medio de eventos de cuatro a ocho y aumenta las horas de 354,688 a 530,216. El desembolso monetario medio baja de 834,37 a 726,08 UM, pero los recursos disponibles permiten sostener más actividad. Con la valoración del tiempo utilizada, ese trabajo adicional supera la mejora de utilidad operativa. Por tanto, abaratar una campaña no implica automáticamente mejorar la utilidad ajustada bajo una política que continúa ejecutándola mientras dispone de recursos.

E presenta la mayor media de clientes en 23 de las 25 condiciones. Con presupuesto 600 o costos de campaña duplicados, B alcanza 19,53 clientes frente a 19,17 de E. Esa diferencia de 0,36 es un orden de medias; no se interpreta aquí como superioridad concluyente.

## 4. La capacidad basal no limita la adquisición

Cambiar la capacidad de 20 a 5 o 40 reproduce exactamente los cinco resultados comprobados por semilla y política: clientes nuevos, utilidad operativa, utilidad ajustada, eventos y horas, en los **1.400 casos** de esos dos escenarios. En el basal, ninguna venta se pierde por capacidad y el máximo mensual observado es cinco.

Con capacidad de una venta mensual se registran 1.527 oportunidades de compra no atendidas, distribuidas entre 479 de las 700 ejecuciones. Con necesidad y conversión altas y capacidad uno se registran 6.132 oportunidades no atendidas en 633 ejecuciones. Son oportunidades acumuladas, no necesariamente personas distintas. G introduce 139 y 425 meses de pausa por capacidad, respectivamente.

El escenario combinado sin restricción eleva la media de clientes de E a 67,82 y de G a 35,90. Con capacidad uno, esas medias bajan a 35,07 y 23,48. La ausencia de sensibilidad alrededor del basal no debe confundirse con irrelevancia de la capacidad bajo mayor presión de demanda.

## 5. Panorama de los 25 escenarios

Todas las cifras son medias al mes 60. La utilidad ajustada descuenta tiempo a 2 UM/h. La columna final ordena medias de clientes; no resume intervalos ni pruebas de superioridad. A alcanza la mayor utilidad ajustada media en todas las filas, con empate A/C en `reward_zero`.

| Condición | Clientes E | Clientes G | Utilidad ajustada G | Mayor media de clientes |
|---|---:|---:|---:|:---:|
| baseline | 26,79 | 17,41 | -885,53 | E |
| conversion_low | 15,93 | 11,96 | -1215,94 | E |
| conversion_high | 37,46 | 21,97 | -641,12 | E |
| need_low | 10,46 | 7,72 | -1364,08 | E |
| need_high | 46,35 | 25,33 | -513,05 | E |
| campaign_cost_low | 34,93 | 17,41 | -626,67 | E |
| campaign_cost_high | 19,17 | 17,26 | -1302,21 | B |
| reward_zero | 21,28 | 17,49 | -892,22 | E |
| reward_low | 24,90 | 17,37 | -881,87 | E |
| reward_high | 27,98 | 17,42 | -888,33 | E |
| budget_low | 19,17 | 17,26 | -818,57 | B |
| budget_high | 34,93 | 17,41 | -885,53 | E |
| capacity_stress | 19,42 | 14,82 | -1073,52 | E |
| capacity_low | 26,79 | 17,41 | -885,53 | E |
| capacity_high | 26,79 | 17,41 | -885,53 | E |
| segregation_high | 23,53 | 15,95 | -955,19 | E |
| segregation_low | 29,59 | 19,10 | -800,99 | E |
| acquisition_threshold_low | 26,79 | 13,62 | -513,55 | E |
| acquisition_threshold_high | 26,79 | 19,45 | -1158,15 | E |
| activity_threshold_low | 26,79 | 17,42 | -885,69 | E |
| activity_threshold_high | 26,79 | 17,40 | -884,91 | E |
| evidence_threshold_low | 26,79 | 17,81 | -944,81 | E |
| evidence_threshold_high | 26,79 | 17,29 | -874,45 | E |
| high_demand_conversion | 67,82 | 35,90 | -443,76 | E |
| high_demand_capacity_stress | 35,07 | 23,48 | -522,57 | E |

Los escenarios de segregación mantienen constante el grado esperado de la red en 9,92, pero permiten cambiar su realización y composición. Una menor segregación eleva la media de clientes de E de 26,79 a 29,59 y de G de 17,41 a 19,10; una mayor segregación las reduce a 23,53 y 15,95. La inferencia se limita a este mecanismo de red y a los niveles ensayados.

## Implicación para el artículo y siguiente experimento

Los resultados permiten formular tres mecanismos para la discusión: (i) mayor difusión puede coexistir con menor utilidad cuando se contabiliza el tiempo; (ii) el orden de reglas puede impedir que una política adaptativa use mecanismos para los que cumple condiciones numéricas; y (iii) una restricción de capacidad puede permanecer inactiva en el basal y modificar los resultados bajo mayor demanda.

El siguiente experimento recomendado es una comparación controlada del **orden de prioridad de las reglas de G**, manteniendo la versión actual como referencia, los mismos parámetros y semillas emparejadas. Debe fijarse antes de ejecutarse y contrastar si permitir incentivos antes de la adquisición cambia las decisiones, la captación y los costos. No se debe ajustar la regla buscando que G resulte ganadora. Ese experimento **no forma parte de esta entrega**.

Todavía quedan fuera de este bloque el timing de eventos, otras intensidades de WOM orgánico, reglas alternativas de prioridad y una exploración global de interacciones. Tampoco hay calibración con datos reales, compras repetidas ni evidencia de validez externa. Los intervalos estrechos no corrigen esas limitaciones estructurales.

## Archivos que permiten verificar los resultados

- `Article_7_Protocolo_sensibilidad_v0.3.md`: matriz y decisiones de análisis fijadas antes de estas ejecuciones.
- `Article_7_Sensibilidad_Python_v0.3.zip`: motor, ejecutor, análisis, pruebas, configuraciones, referencia basal y notebook.
- `Article_7_Sensibilidad_datos_v0.3_parte01.zip` a `parte08.zip`: registros completos; extraer las ocho partes en el mismo directorio.
- `Article_7_Sensibilidad_analisis_v0.3.zip`: tablas, contrastes, diagnósticos, resúmenes por ejecución y manifiestos.
- `Article_7_Sensibilidad_figuras_v0.3.zip`: dos figuras en PNG y SVG. Sus valores son medias, no intervalos.
- `Article_7_Verificacion_sensibilidad_v0.3.md`: evidencia de verificación y alcance de las comprobaciones.

Fuentes numéricas del informe: `policy_summary.csv`, `paired_changes_from_baseline.csv`, `time_break_even.csv`, `scenario_diagnostics.csv` y `adaptive_action_counts.csv`. La clave global de registros es `(scenario, run_id)`; el identificador local se repite entre escenarios.
