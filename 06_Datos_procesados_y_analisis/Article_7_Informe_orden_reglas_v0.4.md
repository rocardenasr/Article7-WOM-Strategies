# Article 7 — Comparación de órdenes de reglas adaptativas v0.4

**Fecha:** 26 de septiembre de 2026. **Motor:** v0.2.0, sin modificaciones. **Intervención y análisis:** v0.4.

## Conclusión

Reordenar las reglas cambia la composición de las acciones, pero **ningún orden alternativo demuestra una mejora en clientes nuevos ni en utilidad ajustada frente al original con las bandas simultáneas fijadas para los 46 contrastes primarios**. Esto no demuestra equivalencia ni ausencia de efectos: los intervalos permiten mejoras y deterioros de distinta magnitud.

Adelantar incentivos al primer lugar entre las reglas de acción (6–4–5–7) aumenta su uso de ocho a 293 meses en las 100 réplicas. Los clientes nuevos medios cambian de 17,41 a 17,31 y la utilidad ajustada de −885,53 a −849,64 UM. La incertidumbre no permite afirmar una mejora en ninguno de los dos resultados primarios. Por su parte, adelantar eventos manteniendo adquisición después (5–4–6–7) incrementa los eventos y reduce la utilidad ajustada.

La evidencia apoya un mecanismo de **precedencia de reglas**: una condición puede cumplirse y, aun así, no determinar la acción porque otra regla se evalúa primero. También muestra que aumentar el uso de una campaña no asegura mejorar el desempeño agregado.

## Diseño y verificación

Se enumeraron las 24 permutaciones de cuatro reglas, conservando las condiciones y la factibilidad de cada una:

| Regla | Mecanismo |
|---|---|
| 4 | Adquisición por referidos cuando clientes o promotores están por debajo de sus umbrales |
| 5 | Eventos ante redundancia alta o alcance limitado, con evidencia suficiente |
| 6 | Incentivos ante baja actividad y redundancia baja, con historia y evidencia suficientes |
| 7 | Continuación de referidos durante el inicio o con rendimiento marginal suficiente |

Las comprobaciones previas de evento comprometido, capacidad y agotamiento/factibilidad permanecieron primero; la pausa por defecto permaneció al final. El protocolo contiene las desigualdades y agrupaciones lógicas exactas. En 18 permutaciones, la regla 7 deja explícitamente de ser la última regla de acción.

Se ejecutaron **2.400 trayectorias**: 2.300 de órdenes alternativos y 100 reproducciones del original. Las 100 reproducciones coinciden con los resúmenes y las **6.000 decisiones mensuales** archivadas. Se reutilizaron 600 casos de A–F para contextualizar: 3.000 casos alternativa–semilla en el análisis conjunto. La referencia reproducida no se cuenta dos veces como muestra independiente.

Cada alternativa usa 100 semillas emparejadas (2001–2100), 600 personas —incluidos 15 clientes iniciales— y 60 meses. Se conservan los parámetros basales: presupuesto 1.200 UM, capacidad 20 y tiempo valorado a 2 UM/h. Todas las cifras de clientes del informe excluyen los 15 iniciales. No se volvieron a ejecutar aquí los 24 escenarios de parámetros de v0.3.

Las **39 pruebas** finalizaron sin fallos. Cada una de las 2.400 ejecuciones superó la auditoría, con 144.000 periodos revisados. El motor original y su ejecutor mantienen los hashes archivados. Se verificaron las guardas, la omisión de reglas inviables, la neutralidad en políticas fijas y la detección de decisiones adulteradas.

## Resultados de los seis órdenes que conservan la regla 7 al final

Estos seis órdenes permiten observar cambios de adquisición, eventos e incentivos manteniendo continuación de referidos como última opción. Las cifras de clientes, utilidad, horas y eventos son medias por ejecución; la columna de incentivos es un total.

| Orden | Clientes nuevos | Utilidad ajustada (UM) | Horas | Meses con incentivos¹ | Eventos medios |
|---|---:|---:|---:|---:|---:|
| 4–5–6–7 | 17,41 | -885,53 | 253,52 | 8 | 0,17 |
| 4–6–5–7 | 17,41 | -885,53 | 253,52 | 8 | 0,17 |
| 5–4–6–7 | 17,90 | -975,30 | 243,57 | 8 | 2,33 |
| 5–6–4–7 | 18,30 | -964,72 | 236,03 | 264 | 2,75 |
| 6–4–5–7 | 17,31 | -849,64 | 243,00 | 293 | 0,31 |
| 6–5–4–7 | 18,33 | -961,31 | 235,55 | 265 | 2,73 |

¹ Meses con incentivos sumados en las 100 réplicas: denominador de 6.000 meses por orden. Por ejemplo, ocho meses equivalen al 0,13% y 293 al 4,88%.

Las diferencias siguientes se calculan dentro de cada semilla. Las bandas corresponden a la familia completa de 46 contrastes primarios —23 órdenes alternativos por dos resultados—, aunque aquí se muestren solo cinco órdenes.

| Orden vs. 4–5–6–7 | Cambio en clientes [banda 95%] | Cambio en utilidad ajustada [banda 95%] |
|---|---:|---:|
| 4–6–5–7 | 0,00 [0,00; 0,00] | 0,00 [0,00; 0,00] |
| 5–4–6–7 | 0,49 [-0,38; 1,36] | -89,78 [-169,96; -9,60] |
| 5–6–4–7 | 0,89 [-0,44; 2,22] | -79,19 [-175,98; 17,60] |
| 6–4–5–7 | -0,10 [-1,18; 0,98] | 35,88 [-42,73; 114,50] |
| 6–5–4–7 | 0,92 [-0,41; 2,25] | -75,79 [-174,28; 22,71] |

### Adelantar incentivos sí modifica la selección de acciones

Con 4–5–6–7, 563 meses cumplen las condiciones numéricas de incentivos y la acción es factible, pero la adquisición por referidos se selecciona antes en 555; solo ocho usan incentivos, repartidos en ocho ejecuciones.

Con 6–4–5–7, 293 meses cumplen esas condiciones y factibilidad sin una guarda previa; los 293 seleccionan incentivos, repartidos en 98 ejecuciones. No debe interpretarse que faltan 270 meses: las acciones distintas modifican estados futuros y, por tanto, la frecuencia de las condiciones cambia endógenamente. El análisis cuenta los estados propios de cada trayectoria, no aplica todas las reglas a una historia artificialmente fija.

Pese a la mayor utilización de incentivos, el cambio medio en captación es −0,10 clientes y el cambio en utilidad ajustada es +35,88 UM. Sus bandas simultáneas incluyen cero. Intercambiar únicamente eventos e incentivos, dejando adquisición primero (4–6–5–7), reproduce exactamente las series mensuales de 4–5–6–7 en las 100 semillas observadas.

### Adelantar eventos puede aumentar los costos

El orden 5–4–6–7 aumenta los eventos medios de 0,17 a 2,33 y los costos monetarios medios de 517,76 a 631,37 UM. La captación pasa de 17,41 a 17,90, pero el contraste simultáneo de clientes incluye cero. La utilidad ajustada cae 89,78 UM, con banda simultánea [−169,96; −9,60].

Los órdenes 5–4–7–6, 5–7–4–6 y 5–7–6–4 también tienen bandas de utilidad ajustada por debajo de cero y generan series mensuales idénticas entre sí. Son tres etiquetas de una misma trayectoria observada por semilla; no constituyen tres confirmaciones independientes del mecanismo.

## Enumeración completa y órdenes con resultados coincidentes

| Orden | Clientes nuevos | Utilidad ajustada (UM) | Horas | Meses con incentivos¹ | Eventos medios |
|---|---:|---:|---:|---:|---:|
| 4–5–6–7 | 17,41 | -885,53 | 253,52 | 8 | 0,17 |
| 4–5–7–6 | 17,49 | -892,22 | 254,84 | 0 | 0,20 |
| 4–6–5–7 | 17,41 | -885,53 | 253,52 | 8 | 0,17 |
| 4–6–7–5 | 17,39 | -882,45 | 254,00 | 9 | 0,09 |
| 4–7–5–6 | 17,48 | -890,56 | 256,00 | 0 | 0,10 |
| 4–7–6–5 | 17,48 | -890,56 | 256,00 | 0 | 0,10 |
| 5–4–6–7 | 17,90 | -975,30 | 243,57 | 8 | 2,33 |
| 5–4–7–6 | 17,96 | -982,14 | 244,88 | 0 | 2,36 |
| 5–6–4–7 | 18,30 | -964,72 | 236,03 | 264 | 2,75 |
| 5–6–7–4 | 18,30 | -964,72 | 236,03 | 264 | 2,75 |
| 5–7–4–6 | 17,96 | -982,14 | 244,88 | 0 | 2,36 |
| 5–7–6–4 | 17,96 | -982,14 | 244,88 | 0 | 2,36 |
| 6–4–5–7 | 17,31 | -849,64 | 243,00 | 293 | 0,31 |
| 6–4–7–5 | 17,33 | -837,67 | 243,75 | 298 | 0,08 |
| 6–5–4–7 | 18,33 | -961,31 | 235,55 | 265 | 2,73 |
| 6–5–7–4 | 18,33 | -961,31 | 235,55 | 265 | 2,73 |
| 6–7–4–5 | 17,33 | -837,67 | 243,75 | 298 | 0,08 |
| 6–7–5–4 | 17,69 | -871,54 | 243,09 | 290 | 0,71 |
| 7–4–5–6 | 17,48 | -890,56 | 256,00 | 0 | 0,10 |
| 7–4–6–5 | 17,48 | -890,56 | 256,00 | 0 | 0,10 |
| 7–5–4–6 | 17,80 | -899,41 | 250,64 | 0 | 0,62 |
| 7–5–6–4 | 17,80 | -899,41 | 250,64 | 0 | 0,62 |
| 7–6–4–5 | 17,48 | -890,56 | 256,00 | 0 | 0,10 |
| 7–6–5–4 | 17,80 | -899,41 | 250,64 | 0 | 0,62 |

¹ La columna de incentivos suma meses en 100 réplicas; las demás columnas son medias.

Las 24 permutaciones generan **12 grupos de series mensuales idénticas** en estas semillas. La agrupación se verifica por igualdad de los archivos de series, sin incluir el identificador de la regla seleccionada, que puede diferir cuando dos reglas conducen a la misma acción. No se afirma equivalencia para cualquier estado, parámetro o semilla.

| Grupo observado | Órdenes con series mensuales idénticas |
|---:|---|
| 1 | 4–5–6–7, 4–6–5–7 |
| 2 | 4–5–7–6 |
| 3 | 4–6–7–5 |
| 4 | 4–7–5–6, 4–7–6–5, 7–4–5–6, 7–4–6–5, 7–6–4–5 |
| 5 | 5–4–6–7 |
| 6 | 5–4–7–6, 5–7–4–6, 5–7–6–4 |
| 7 | 5–6–4–7, 5–6–7–4 |
| 8 | 6–4–5–7 |
| 9 | 6–4–7–5, 6–7–4–5 |
| 10 | 6–5–4–7, 6–5–7–4 |
| 11 | 6–7–5–4 |
| 12 | 7–5–4–6, 7–5–6–4, 7–6–5–4 |

En las 12 permutaciones que sitúan continuación de referidos (7) antes de incentivos (6), no se seleccionan incentivos. Bajo los umbrales basales, cumplir la condición de incentivos implica redundancia <0,60 y, por tanto, rendimiento marginal >0,40, superior al umbral 0,25 de continuación. Si referidos es factible, la regla 7 puede resolver antes la decisión. Esta relación explica la supresión observada; no se extiende sin condiciones a otros costos, umbrales o restricciones.

La mayor utilidad ajustada media entre los órdenes de G es −837,67 UM, compartida por 6–4–7–5 y 6–7–4–5. La mejora media frente al original es **47,86 [-31,89; 127,60] UM**: la banda incluye cero. Su captación media es 17,33. La mayor captación media es 18,33, compartida por 6–5–4–7 y 6–5–7–4, y tampoco demuestra mejora frente al original con la banda simultánea. Estos máximos descriptivos no identifican órdenes óptimos validados.

## Comparación contextual con las políticas fijas

WOM orgánico (A) mantiene una utilidad ajustada media de 30,40 UM y 3,80 clientes nuevos. Incentivos fijos (C) producen −38,74 UM y 8,00 clientes; mezcla fija (E), −1.329,43 UM y 26,79 clientes. Ninguna permutación de G cambia ese patrón de intercambio entre captación y utilidad: todas las medias de utilidad ajustada de G siguen siendo negativas y sus medias de captación permanecen por debajo de E en esta configuración.

Esta comparación contextual reutiliza las mismas 100 semillas y se reporta como exploratoria. No equivale a una comparación causal con empresas reales ni demuestra validez empírica de las políticas.

## Incertidumbre y límites

Se utilizaron 5.000 remuestreos por bloques de semillas, compartidos entre alternativas y métricas. Las tablas conservan intervalos percentiles marginales y errores estándar Monte Carlo. Para los 46 contrastes primarios se calcularon bandas simultáneas aproximadas mediante el máximo absoluto de desviaciones de medias estandarizadas por su error estándar emparejado observado. El valor crítico obtenido fue 2,9675. Dos contrastes son exactamente constantes y tienen intervalo puntual; los otros 44 entran en el máximo.

Las bandas describen incertidumbre Monte Carlo condicionada al modelo. No incluyen incertidumbre de parámetros reales ni garantizan cobertura exacta. Los intervalos que contienen cero no demuestran equivalencia. El protocolo se fijó después de conocer el basal y la sensibilidad v0.3, pero antes de simular los órdenes.

Se exploraron exhaustivamente los órdenes de estas cuatro reglas únicamente bajo parámetros basales. No se estudiaron nuevas condiciones de activación, otras funciones objetivo, cambios de timing de eventos ni interacciones del orden con los escenarios de parámetros anteriores. La compra única y los demás supuestos estructurales del motor permanecen vigentes.

## Implicación para el manuscrito

Conviene conservar 4–5–6–7 como referencia y presentar este bloque como comprobación del mecanismo de precedencia. Los resultados respaldan que el orden afecta qué campañas se activan, pero no justifican reemplazar la referencia por un supuesto orden ganador. La redacción debe separar cambio en composición de acciones de mejora en desempeño.

El siguiente paso útil es integrar estos resultados en métodos, resultados y discusión del artículo. Si se desea proponer posteriormente un orden preferido, primero habrá que definir un criterio de selección y validarlo con semillas o configuraciones independientes; este informe no realiza esa validación.

## Trazabilidad y entrega

- Diseño congelado SHA-256: `333e57b0af2767be61a8d5bbf163cdd8ad6fc069d026fbde5eb0505bb527fd5d`.
- Ejecución UTC: 2026-09-26T09:47:08.768995+00:00 a 2026-09-26T09:48:46.020340+00:00; 97,25 segundos en cuatro procesos, excluyendo análisis y empaquetado.
- Tablas: `alternative_summary.csv`, `paired_order_contrasts.csv`, `exploratory_fixed_policy_contrasts.csv`, `mechanism_totals.csv` y `observed_monthly_series_groups.csv`.
- Figuras: `order_effects` (bandas simultáneas) y `order_actions_and_outcomes` (acciones y medias), en PNG y SVG.
- Dos partes de datos contienen todas las trazas y las condiciones iniciales compartidas; se extraen juntas bajo `rule_orders_v0.4/`.
- Los ocho archivos de trazas v0.3 pendientes se subieron a la carpeta de resultados brutos y se verificaron tras la autorización explícita del usuario. Sus enlaces figuran en el índice de esta entrega.
