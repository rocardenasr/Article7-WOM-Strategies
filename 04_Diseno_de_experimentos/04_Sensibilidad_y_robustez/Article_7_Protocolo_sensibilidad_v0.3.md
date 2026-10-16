# Article 7 — Protocolo de sensibilidad v0.3

**Diseño fijado después de observar el escenario basal y antes de ejecutar esta sensibilidad.** No es un preregistro anterior al modelo ni una calibración empírica. Se preserva el motor Python v0.2.0 y sus reglas.

## Objetivo

Examinar si las diferencias entre las siete políticas dependen de supuestos sobre conversión, necesidad, costos, recompensa, recursos, estructura y umbrales de la política adaptativa. El análisis permitirá diagnosticar límites de interpretación y cambios de orden; no se buscarán parámetros que hagan ganar una política determinada.

## Diseño y unidad de análisis

Se ejecutarán 24 escenarios nuevos × siete políticas × 100 repeticiones = **16.800 simulaciones nuevas**. Se reutilizarán los 700 resultados del escenario basal existente como referencia: 25 escenarios y 17.500 combinaciones escenario–política–semilla. Cada escenario tendrá 600 agentes y 60 meses. El piloto anterior de 70 ejecuciones no se incluirá.

Las semillas serán 2001–2100, compartidas entre políticas y escenarios. Los sorteos indexados permitirán comparaciones emparejadas y no dependerán del orden ni del número de procesos. En los escenarios de red se comparten los sorteos potenciales de enlaces, aunque su realización puede cambiar al aplicar otras probabilidades. El grado esperado se mantendrá en 9,92; la densidad realizada podrá variar.

Las repeticiones independientes son las semillas. Meses, personas, enlaces, escenarios y políticas de la misma semilla no se interpretarán como observaciones independientes. Las 16.800 ejecuciones no equivalen a 16.800 replicaciones independientes para un mismo contraste.

## Matriz de escenarios

Cada fila modifica únicamente el bloque indicado. La referencia utiliza el archivo `baseline.json` de v0.2.

| Escenario | Bloque | Cambios |
|---|---|---|
| conversion_low | conversion | conversion=0.15 |
| conversion_high | conversion | conversion=0.35 |
| need_low | need | need_p=0.15 |
| need_high | need | need_p=0.45 |
| campaign_cost_low | campaign_cost | referral_cost=0.5; event_fixed=30; event_per_person=0.5 |
| campaign_cost_high | campaign_cost | referral_cost=2; event_fixed=120; event_per_person=2 |
| reward_zero | reward | reward=0 |
| reward_low | reward | reward=1 |
| reward_high | reward | reward=4 |
| budget_low | budget | budget=600 |
| budget_high | budget | budget=2400 |
| capacity_stress | capacity | capacity=1 |
| capacity_low | capacity | capacity=5 |
| capacity_high | capacity | capacity=40 |
| segregation_high | network | p_out=0.001; p_in=0.0951515 |
| segregation_low | network | p_out=0.01; p_in=0.049697 |
| acquisition_threshold_low | adaptive_acquisition | threshold_customers=24; threshold_promoters=8 |
| acquisition_threshold_high | adaptive_acquisition | threshold_customers=36; threshold_promoters=12 |
| activity_threshold_low | adaptive_activity | threshold_activity=0.16 |
| activity_threshold_high | adaptive_activity | threshold_activity=0.24 |
| evidence_threshold_low | adaptive_evidence | min_exposures=16 |
| evidence_threshold_high | adaptive_evidence | min_exposures=24 |
| high_demand_conversion | joint | need_p=0.45; conversion=0.35 |
| high_demand_capacity_stress | joint | need_p=0.45; conversion=0.35; capacity=1 |

Los costos bajo/alto escalan conjuntamente los desembolsos de referidos y eventos a 0,5×/2×. No cambian el pago de recompensas, las horas requeridas ni la valoración del tiempo. La recompensa se estudia por separado porque modifica simultáneamente activación y desembolsos. Los umbrales de adquisición cambian conjuntamente C/P: no identifican el efecto individual de cada umbral. Los escenarios de red cambian dos probabilidades para aislar segregación respecto al grado esperado. Las dos filas `joint` son escenarios combinados de demanda/conversión y demanda/conversión/capacidad; no constituyen un factorial completo ni permiten identificar todas las interacciones.

Capacidad 1 es una condición diagnóstica adicional: la capacidad basal no resultó vinculante. Los escenarios 5 y 40 conservan los niveles originalmente propuestos. No se añaden reglas de adaptación ni nuevas políticas tras conocer los resultados de esta sensibilidad.

## Valoración del tiempo sin nuevas simulaciones

Se evaluarán 0, 2 y 4 UM por hora mediante `utilidad_ajustada(w) = utilidad_operativa − w × horas`. En este modelo w no entra en ninguna regla de comportamiento ni restricción; por tanto, recalcularlo sobre la misma trayectoria es exacto. Se verificará esta equivalencia mediante ejecución directa con valores alternativos. Habrá 52.500 evaluaciones contables (25 × 7 × 100 × 3), que **no** se contarán como nuevas simulaciones ni réplicas independientes.

Se estimará también el punto de igualdad de utilidad entre cada campaña y la política orgánica por la razón de diferencias medias de utilidad operativa y horas, cuando el denominador sea positivo. Es un umbral condicionado a supuestos y puede ser negativo; no es un valor óptimo empírico.

## Análisis fijado

Resultados principales: clientes nuevos y utilidad ajustada al mes 60 con w=2. Complementarios: utilidad operativa, costos, horas, alcance, eventos, demanda no atendida y acciones de G.

Se reportarán diferencias emparejadas de cada política frente a A y de G frente a B–F dentro de cada escenario. Se estimarán cambios de cada política frente a su referencia basal, conservando el emparejamiento por semilla. Los indicadores de G incluirán frecuencia de cada acción y proporción de ejecuciones con eventos e incentivos.

Intervalos percentiles del 95% con 5.000 remuestreos de bloques de semillas, semilla bootstrap 730003. Se reutilizarán los mismos índices entre escenarios y políticas. Se informará error estándar Monte Carlo. Los intervalos son marginales, no simultáneos. No se utilizarán p-valores ni se interpretará que ordenar medias demuestra superioridad universal o separación estadística de todas las alternativas. Se conservarán todos los escenarios, también los que no cambien el resultado.

La sensibilidad es local por bloques, más dos condiciones conjuntas. No calcula índices globales de Sobol, no cubre el espacio completo de parámetros y no valida empíricamente los mecanismos. El timing de eventos, el orden de prioridad de reglas, la intensidad orgánica y otras extensiones siguen fuera de este bloque.

## Verificación y conservación

Se comprobarán validez de configuraciones, conservación del grado esperado, neutralidad conductual del costo de tiempo y ausencia de efectos de parámetros exclusivos de G sobre políticas fijas. Se conservarán las 24 pruebas anteriores y se añadirán cinco pruebas específicas.

Cada ejecución evaluará invariantes y auditoría de registros individuales. Se guardarán configuraciones, semillas, series mensuales, decisiones, solicitudes, recomendaciones, pagos, eventos, redes iniciales, enlaces nuevos, resúmenes y auditorías. Los archivos agregados incluirán identificador de escenario; las trazas individuales lo heredan de su carpeta y manifiesto. La clave global de análisis es `(scenario, run_id)` porque los run_id locales se repiten entre escenarios.

Una violación detendrá el proceso y se documentará. Los resultados inesperados o un mal desempeño de G no son razones para alterar el diseño. Los cambios futuros constituirán otra versión.

SHA-256 del diseño JSON congelado: `690d1684aec61da27e6c31eb03de9823f02948308e74b428eefa6bf6e6bacee4`.
