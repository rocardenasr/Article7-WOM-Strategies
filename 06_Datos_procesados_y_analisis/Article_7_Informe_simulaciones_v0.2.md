# Article 7 — Implementación, verificación y simulaciones en Python v0.2

**Estado:** modelo ejecutable, verificación aprobada y comparación basal completada.  
**Trabajo:** 25 de septiembre de 2026, Colombia; las marcas UTC de ejecución pueden mostrar 26 de septiembre.  
**Base científica:** especificación mínima v0.1 y adenda Python v0.2.  
**Naturaleza:** ejercicio teórico sin calibración empírica.

## 1. Resultado entregado

Se implementaron las siete políticas y se completaron **770 ejecuciones**: 70 de piloto y 700 principales. Todas tienen 600 agentes, seis comunidades y 60 meses. Las **24 pruebas automatizadas** aprobaron. La conciliación de registros individuales con series mensuales y resúmenes aprobó en las 770 ejecuciones: **46.200 registros ejecución–mes**.

El piloto usa semillas 1001–1010 y el principal 2001–2100. Las siete políticas comparten red, atributos y sorteos pertinentes por semilla. Los resultados del piloto no se mezclan con los principales. El modelo no fue recalibrado tras observar el piloto.

## 2. Resultados del experimento principal

Cada fila resume 100 repeticiones. Los intervalos entre corchetes corresponden al 95% mediante 5.000 remuestreos de repeticiones completas. Reflejan incertidumbre Monte Carlo condicionada a este escenario, no incertidumbre empírica. Son intervalos marginales, no simultáneos para todas las comparaciones.

La utilidad operativa descuenta costos de servicio y campañas. La utilidad ajustada descuenta además 2 unidades monetarias por hora de marketing. Los 15 clientes iniciales no cuentan como nuevas adquisiciones ni generan ingresos en el período simulado.

| Política | Nuevos clientes, media [IC95%] | Utilidad operativa media (UM) | Utilidad ajustada, media [IC95%] (UM) | Gasto de campañas medio (UM) |
|---|---:|---:|---:|---:|
| A · Orgánica | 3.80 [3.39, 4.24] | 30.40 | 30.40 [27.12, 33.92] | 0.00 |
| B · Referidos | 20.70 [19.37, 22.06] | -554.40 | -1274.40 [-1285.04, -1263.52] | 720.00 |
| C · Incentivos | 8.00 [7.38, 8.61] | 48.00 | -38.74 [-41.26, -36.28] | 16.00 |
| D · Eventos | 8.94 [8.26, 9.64] | -744.38 | -987.56 [-991.24, -983.95] | 815.90 |
| E · Mezcla | 26.79 [25.28, 28.34] | -620.05 | -1329.43 [-1336.83, -1321.98] | 834.37 |
| F · Secuencia | 17.07 [16.03, 18.10] | -465.96 | -846.94 [-852.77, -840.97] | 602.52 |
| G · Adaptativa | 17.41 [16.63, 18.20] | -378.48 | -885.53 [-952.35, -819.27] | 517.76 |

## 3. Qué indican estos resultados

**Existe un conflicto entre adquisición y rentabilidad bajo los supuestos basales.** La mezcla alcanza el mayor número medio de clientes nuevos, pero sus costos monetarios y de tiempo producen la menor utilidad ajustada media. La política orgánica crece poco y tiene la mayor utilidad ajustada media. Los incentivos mejoran el margen operativo medio frente a la política orgánica, pero pierden esa ventaja cuando se incluye el costo administrativo de tiempo.

**La política adaptativa no domina las alternativas.** Su comportamiento responde a los umbrales fijados, pero la regla de adquisición tiene prioridad y frecuentemente mantiene solicitudes de referidos. Las decisiones de G en las 6.000 observaciones ejecución–mes fueron:

| Acción | Meses seleccionada | Porcentaje |
|---|---:|---:|
| referral | 4187 | 69.78% |
| pause | 1771 | 29.52% |
| event | 17 | 0.28% |
| hold_event | 17 | 0.28% |
| incentive | 8 | 0.13% |

La política G realizó eventos en 14 de 100 ejecuciones y cambió de acción al menos una vez en 76. Las 8 selecciones de incentivos equivalen a apenas 0,13% de sus meses. Por ello, este escenario caracteriza sobre todo una regla de referidos con pausas; ofrece poca evidencia sobre una combinación adaptativa sostenida de las tres intervenciones. Este es un diagnóstico del escenario y de los umbrales, no una demostración de que los incentivos carezcan de utilidad.

**La capacidad no fue una restricción activa.** No hubo demanda no atendida en las 700 ejecuciones. El máximo de ventas mensuales fue 5 frente a capacidad 20, una utilización máxima de 25%. El módulo de capacidad sí se verificó con condiciones extremas, pero estos resultados basales no permiten explicar decisiones frente a congestión.

El crecimiento es lento: la mayor media de nuevas adquisiciones es 26,79 sobre 585 personas inicialmente no clientes. El escenario no representa una empresa típica ni una estimación del rendimiento de campañas reales.

## 4. Decisiones y límites que deben acompañar al manuscrito

- Compra única, necesidad mensual independiente y compra solo con una exposición nueva en ese mes. No hay compras por recuerdo ni recompras.
- Presupuesto cerrado de campañas; las ventas no amplían automáticamente los recursos de marketing.
- Los eventos invitan a clientes y a sus vecinos. Pueden crear enlaces entre asistentes, pero no alcanzar un componente inicialmente desconectado por medios externos.
- La empresa observa un registro completo de interacciones. No se representa error de medición en los indicadores adaptativos.
- Los costos de tiempo son supuestos económicos y alteran la comparación de políticas; deben analizarse por separado.
- La atribución por exposición decisiva es una regla contable. No identifica efectos causales independientes de cada canal en presencia de trayectorias e interacciones.
- Los errores de programación cubiertos por pruebas se distinguen de la plausibilidad de supuestos, que requiere revisión conceptual de los investigadores.

## 5. Siguiente paso científico recomendado

Antes de redactar conclusiones generales, registrar una matriz de sensibilidad que permita examinar los mecanismos poco activos. Priorizar: (1) necesidad y conversión; (2) costos de campañas y tiempo; (3) umbrales y prioridad de reglas adaptativas; (4) capacidad baja que pueda vincular demanda y atención; (5) presupuesto y modularidad manteniendo grado esperado; y (6) timing de eventos e intensidad de recompensas.

Los valores ya propuestos en v0.1 sirven como punto de partida. Deben fijarse antes de ejecutar cada bloque y reportarse aunque contradigan las expectativas. No se recomienda elegir umbrales por conseguir que G ocupe el primer lugar. La comparación basal es una base reproducible; los experimentos de sensibilidad y robustez del artículo siguen pendientes.

Un mapa de decisiones por escenarios puede construirse cuando existan esas condiciones. Afirmar que una acción es la mejor para un mismo estado requeriría comparar acciones desde estados clonados con futuros aleatorios emparejados; todavía no se ejecutó ese diseño.

## 6. Archivos y reproducción

- `Article_7_Python_v0.2.zip`: código, configuración, 24 pruebas, notebook, especificación y adenda.
- `Article_7_Colab_v0.2.ipynb`: interfaz de ejecución reproducible para el estudiante.
- `Article_7_Piloto_70_v0.2.zip`: datos originales y auditoría del piloto.
- `Article_7_Principal_700_v0.2.zip`: datos originales, auditoría y manifiestos del experimento principal.
- `Article_7_Analisis_v0.2.zip`: tablas, diferencias emparejadas y diagnósticos.
- `Article_7_Figuras_v0.2.zip`: figuras PNG y SVG editables.
- `Article_7_Verificacion_v0.2.md`: alcance, resultados y evidencia de las pruebas.

Las instrucciones del README reproducen pruebas, simulación y análisis. El notebook fue validado sintácticamente; su interacción de carga en Google Colab no se probó en una sesión de Colab. Los comandos subyacentes sí se ejecutaron en el entorno Python registrado. Se conservaron los registros completos, incluidos solicitudes fallidas y ofertas sin pago.

SHA-256 de `model.py`: `e12ba5f7aee01d63f23cce62751b383d77f2651eeb99aa5608955bf9cb3a4441`. Los manifiestos contienen el hash del ejecutor, la configuración, el entorno y las semillas. Los tiempos de ejecución y las cabeceras de compresión pueden cambiar al reproducir archivos; los resultados científicos deben coincidir.
