# Article 7 — Verificación de sensibilidad v0.3

## Resultado

Se ejecutaron 29 pruebas unitarias y de invariantes, sin fallos: las 24 del motor v0.2 y cinco específicas de sensibilidad. El registro íntegro está en `docs/sensitivity_verification_v0.3.txt` dentro del código distribuido.

Las 16.800 ejecuciones nuevas tienen auditoría `passed`, con 1.008.000 periodos mensuales revisados. Las 700 auditorías basales se reutilizan: el agregado contiene 17.500 casos y 1.050.000 periodos revisados, sin duplicados en `(scenario, seed, policy)`. La auditoría comprueba coherencia interna de registros; no demuestra validez empírica del modelo.

## Comprobaciones específicas

| Comprobación | Evidencia | Resultado |
|---|---|---|
| Diseño y dominios | 24 escenarios, siete políticas y 100 semillas por condición | 16.800 casos nuevos válidos |
| Grado esperado en escenarios de red | 99 × p_in + 500 × p_out | 9,92 en ambas condiciones |
| Tiempo como contabilidad | Ejecuciones directas con w=0, 2 y 4 y siete políticas; semilla 2001 | Mismas decisiones y resultados conductuales; cambia solo el ajuste monetario |
| Parámetros exclusivos de G | Comparación completa de registros de A–F en prueba específica | Sin cambios |
| Independencia de políticas fijas en experimentos completos | Seis escenarios de umbrales, A–F, 100 semillas | 3.600 resultados idénticos al basal en cinco indicadores |
| Capacidad alrededor del basal | Capacidad 5 y 40; todas las semillas y políticas | 1.400 resultados idénticos al basal en cinco indicadores |
| Motor y ejecutor basal | SHA-256 contra manifiesto original | Sin modificaciones |
| Integridad de datos | Auditorías por ejecución; todas las claves previstas presentes | 17.500 casos únicos, incluidos 700 reutilizados |

Los cinco indicadores de las comparaciones completas son clientes nuevos, utilidad operativa, utilidad ajustada, eventos y horas. Esta equivalencia agregada no se presenta como una comparación byte a byte de todas las trazas.

## Trazabilidad

- Diseño: `690d1684aec61da27e6c31eb03de9823f02948308e74b428eefa6bf6e6bacee4`.
- Motor `model.py`: `e12ba5f7aee01d63f23cce62751b383d77f2651eeb99aa5608955bf9cb3a4441`.
- Ejecutor basal `experiment.py`: `7cb070cbd86f933d4c339de367f2a82ed562d0cf94f868de5ebdd222afa80049`.
- Ejecutor de sensibilidad: `8aa745b349b02968691731020ef4f284ee92a7a10446c8d6385271f54dae5948`.
- Manifiesto basal: `38d0ad3a07d1cf3bd741d920dd3a5e4fd5a5605220052815546a7639faaf5b59`.
- Inicio UTC: 2026-09-26T02:47:41.396941+00:00.
- Fin UTC: 2026-09-26T02:55:01.621297+00:00.
- Duración de la ejecución de escenarios: 440,20 segundos con cuatro procesos en este entorno. No incluye análisis, escritura del informe ni empaquetado.

Se conservan configuraciones completas por escenario, semillas, condiciones iniciales, recomendaciones, solicitudes, ventas, pagos, decisiones, eventos, series y auditorías. Los archivos individuales heredan el escenario de su carpeta y manifiesto. El índice de datos incluye SHA-256 de cada una de las ocho partes comprimidas.

## Reproducción y límites de verificación

Los comandos del README se ejecutaron localmente con Python 3.12.14 y las dependencias de `requirements-tested.txt`. Se comprobó la sintaxis de todas las celdas Python del notebook. La interfaz interactiva de carga de archivos de Google Colab no se probó desde Colab.

El manifiesto `release_verification.json` incluido en el paquete de análisis registra las comprobaciones finales del archivo distribuido: extracción del paquete de código, 29 pruebas desde esa extracción, reproducción de un caso contra los resultados archivados, sintaxis del notebook e integridad CRC de todos los ZIP. La ausencia de fallos computacionales no resuelve los límites teóricos o empíricos descritos en el informe.
