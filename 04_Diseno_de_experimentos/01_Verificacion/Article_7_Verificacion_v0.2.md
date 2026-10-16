# Article 7 — Informe de verificación v0.2

## Resultado

24 pruebas automatizadas aprobadas mediante `python -m unittest discover -s tests -v`. Se ejecutaron en Python 3.12.14 y NumPy 2.3.5. La suite dura aproximadamente 1–2 segundos en el entorno de ejecución; este tiempo no constituye una garantía de rendimiento.

| Cobertura | Evidencia |
|---|---|
| V01–V04 | Ausencia de clientes, recompensa cero, eventos sin asistentes y comunidades desconectadas. |
| V05–V07 | Dinero comprometido, límites de horas presentes/futuras y capacidad con recompensas solo sobre ventas atendidas. |
| V08–V10 | Ascendencia de referidos y enlaces; activación de enlaces desde el mes siguiente; prioridades y reconstrucción adaptativa. |
| V11–V12 | Ablación completa de campañas y equivalencia de políticas con mecanismos/costos iguales. La contabilidad contrafactual de costos se trata por separado. |
| V13–V14 | Reconciliación independiente de gastos, horas y utilidades; resultados idénticos al invertir el orden de ejecución de políticas. |
| V15–V17 | Exposiciones múltiples, compra única, energía, retraso de nuevos promotores, recursos de mezcla y calendario de secuencia. |
| V18–V20 | Indicadores indefinidos, horizonte sin compromisos pendientes y agregación de registros. |
| Adicional: cadena manual | Red de cuatro personas: una nueva compra por mes, tres compras y utilidad 24 UM, exactamente. |
| Adicional: pago manual | Dos personas: una adquisición por recomendación bajo incentivo, un pago de 2 UM, utilidad operativa 6 y ajustada 5,8. |
| Adicional: red | Copias independientes de una red emparejada y diagnóstico de un triángulo con un nodo aislado. |
| Adicional: entradas | Rechazo de probabilidades, recuentos y configuraciones iniciales inválidas. |

Durante la primera ejecución se detectó un dato de prueba incoherente en V10: 30 clientes en una red de 24 agentes activaban correctamente la regla de mercado agotado. Se corrigió el dato de la prueba a 12 clientes para evaluar la prioridad de eventos. No se cambió la regla del modelo para hacer pasar el caso.

## Auditoría de cada simulación

70/70 ejecuciones piloto y 700/700 principales aprobaron una conciliación adicional independiente de las identidades internas del modelo. Se compararon solicitudes, pagos, eventos, horas, exposiciones, decisiones de compra, ventas y registros mensuales. Se verificaron compradores únicos, pertenencia de extremos de enlaces a los asistentes, ausencia de referidos repetidos por par dirigido y reconstrucción de indicadores históricos de G. Los invariantes internos también se evaluaron en cada mes.

La auditoría no reemplaza una revisión científica externa. Verifica la implementación de supuestos, no la validez empírica de esos supuestos. Capacidad, cero presupuesto y otros extremos fueron verificados mediante pruebas aun cuando no fueran restricciones activas en el experimento basal.

## Reproducibilidad

El ZIP de código contiene la suite, configuración y salida textual de pruebas. Cada paquete de resultados incluye `audit_results.csv`, `run_manifest.csv` y `experiment_manifest.json`. Las semillas no dependen del orden de las políticas y los sorteos comunes se indexan por mecanismo, mes y agentes. El notebook fue comprobado sintácticamente; la interfaz de carga en Colab permanece sin verificación interactiva.
