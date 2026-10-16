# Article 7 — Protocolo de comparación del orden de reglas v0.4

Diseño fijado después de la sensibilidad v0.3 y antes de ejecutar las variantes de orden. Es una intervención computacional sobre el mecanismo de decisión; no es un preregistro anterior al modelo ni una validación con datos reales.

## Pregunta

¿Cómo cambian las decisiones, los clientes nuevos y la utilidad ajustada cuando se altera exclusivamente el orden de las reglas de acción de la política adaptativa G?

## Intervención y referencia

Se comparan todas las 24 permutaciones de estas cuatro reglas, conservando sus condiciones numéricas y pruebas de factibilidad:

| Identificador estable | Acción y condición |
|---|---|
| 4 | Referidos de adquisición cuando clientes <30 o promotores <10 |
| 5 | Evento cuando hay al menos 20 exposiciones recientes y redundancia ≥0,60 o alcance ≤2 comunidades, según la agrupación lógica del motor |
| 6 | Incentivos con historia de tres meses, actividad <0,20, al menos 20 exposiciones y redundancia <0,60 |
| 7 | Referidos de continuación durante la historia inicial o con rendimiento marginal ≥0,25 y al menos 20 exposiciones |

La regla 5 requiere siempre las 20 exposiciones: `exposiciones ≥20 AND (redundancia ≥0,60 OR comunidades ≤2)`. Las ventanas, valores faltantes y desigualdades conservan exactamente el código basal. La regla 7 deja de ser la última opción en 18 permutaciones; ello forma parte explícita de la intervención.

Antes de esas cuatro reglas se mantienen, en este orden: (1) cumplir un evento comprometido, (2) pausar por capacidad, (3) pausar por mercado agotado o falta de acciones factibles. Si ninguna regla de acción selecciona una campaña, se conserva la pausa final (8). Una regla verdadera pero inviable se omite y se examina la siguiente.

La referencia es `G_4567`. Los identificadores 4–7 nombran mecanismos y no posiciones nuevas. El campo histórico `skipped.priority` conserva ese identificador; la posición se obtiene de `rule_order` en el manifiesto. Cada variante se conserva en una carpeta separada y la clave global es `(variant, run_id)`.

## Ejecuciones y controles

- Parámetros: exactamente `configs/baseline.json` de v0.2, incluida capacidad 20, presupuesto 1.200 y tiempo valorado a 2 UM/h.
- Población: 600 personas, incluidos 15 clientes iniciales; horizonte: 60 meses.
- Semillas: 2001–2100, emparejadas entre todas las variantes y políticas fijas.
- Ejecuciones: 24 ×100 = **2.400**, de las cuales **2.300 corresponden a órdenes alternativos nuevos** y 100 reproducen la referencia.
- Se comparan las 100 reproducciones con los resúmenes y 6.000 decisiones archivadas de G. No se cuentan como 100 réplicas independientes adicionales de G.
- Las 600 ejecuciones existentes de A–F se reutilizan para contextualización; no se vuelven a simular en el experimento. La tabla conjunta contiene 3.000 casos por alternativa/semilla.
- Los sorteos y condiciones iniciales son compartidos. El orden no forma parte de ninguna clave aleatoria.
- No se cambian parámetros después de observar resultados. Se conserva la versión original como referencia.

Los cambios se implementan en una subclase que sustituye únicamente la selección de acción. El motor v0.2.0 y su ejecutor se conservan byte a byte. Las políticas A–F y la opción de campañas desactivadas delegan en el comportamiento original.

## Resultados y análisis fijado

Resultados primarios al mes 60: clientes nuevos y utilidad ajustada con w=2. Se calculan los 23 contrastes de cada variante frente a G_4567 para ambos resultados, siempre restando dentro de la misma semilla. Hay 100 unidades independientes por contraste.

Se utilizan 5.000 remuestreos de bloques de semillas, semilla bootstrap 740004, compartiendo índices entre variantes y métricas. Se presentan medias, desviación estándar, error estándar Monte Carlo e intervalos percentiles marginales del 95%.

Para los **46 contrastes primarios**, además se calcula una banda simultánea aproximada del 95% mediante bootstrap max-|t|: en cada remuestreo se divide la desviación de la media remuestreada respecto a la observada por el error estándar emparejado observado; se toma el máximo absoluto entre los contrastes con error estándar positivo. El percentil 95 de esos máximos multiplica el error estándar de cada contraste. Los contrastes exactamente constantes tienen intervalo puntual. Esta banda controla la familia definida bajo el procedimiento bootstrap; no es una garantía de cobertura exacta ni de validez externa.

Resultados secundarios: utilidad operativa, horas, costos, eventos, enlaces nuevos, alcance y demanda no atendida. Los contrastes frente a A–F y los diagnósticos de acciones son exploratorios, con intervalos marginales cuando corresponda. No se calculan p-valores.

Se documentan los meses que cumplen las condiciones numéricas de incentivos, los que además son factibles, los meses resueltos antes por cada regla y los incentivos efectivamente seleccionados. Los conteos provienen de la trayectoria propia de cada variante y no de estados contrafactuales mantenidos artificialmente iguales. También se identifican órdenes que generan series mensuales idénticas en las semillas ensayadas; ello no demuestra equivalencia para todo estado o configuración.

Las seis permutaciones que conservan 7 al final permiten examinar el orden de adquisición, eventos e incentivos sin adelantar continuación de referidos. Las 18 restantes se presentan igualmente. No se eliminan resultados iguales ni desfavorables.

## Verificación y conservación

Se conservan las 29 pruebas existentes y se añaden diez verificaciones de orden: enumeración, equivalencia con la regla original en fronteras, guardas, condiciones simultáneas, omisión por inviabilidad, pausa final, igualdad de trayectorias originales, neutralidad en A–F, detección de decisiones adulteradas y precedencia de continuación frente a incentivos.

Cada ejecución conserva la auditoría contable y de trazas del motor. La reconstrucción del selector usa un oráculo separado, sin llamar al selector de producción. Se conservarán resúmenes, series, decisiones, condiciones iniciales, recomendaciones, solicitudes, pagos, eventos, redes, auditorías y hashes del código y diseño.

## Límites y criterio de interpretación

El experimento enumera exhaustivamente órdenes de estas cuatro reglas **en la configuración basal**. No es una sensibilidad global de parámetros ni compara nuevas reglas, umbrales o funciones objetivo. Los resultados son condicionales al modelo y a sus supuestos ilustrativos.

Ordenar medias no identifica una política universalmente óptima. Elegir un orden por su desempeño y reportarlo como validado en esas mismas semillas introduciría optimismo de selección. Cualquier propuesta posterior de orden preferido deberá distinguir el resultado exploratorio de una evaluación en semillas o condiciones independientes.
