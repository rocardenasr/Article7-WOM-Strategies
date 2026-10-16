# Article 7 — Adenda de implementación en Python v0.2

Fecha de trabajo: 25 de septiembre de 2026 (Colombia). Especificación científica de referencia: v0.1. Versión de código: 0.2.0.

## Decisión de plataforma

Por indicación del profesor, el modelo se implementa y ejecuta íntegramente en Python. NumPy sostiene los atributos y la inicialización; la red se representa mediante conjuntos de vecinos. No se requiere NetLogo, Mesa ni NetworkX. El análisis utiliza pandas y matplotlib. Se emplean pruebas de la biblioteca estándar `unittest`.

La pregunta de investigación, las siete políticas, las reglas de comportamiento, el horizonte de 60 meses y los parámetros ilustrativos de v0.1 se conservan. No se ajustaron valores para producir un ganador esperado. La guía original y la especificación v0.1 se conservan como antecedentes.

## Precisiones necesarias para una ejecución reproducible

1. Las comunidades se numeran desde cero en el código: comunidad 0 corresponde a comunidad 1 de la especificación. Los 600 agentes incluyen los 15 clientes iniciales.
2. La inicialización emplea PCG64 con `SeedSequence([semilla, flujo])`: red 0, ajuste 1, clientes iniciales 2, satisfacción potencial 3, disposición 4 y selección de promotores iniciales 5. Una regeneración excepcional de ajuste usa flujo 10000 más número de intento. No se fuerza conectividad.
3. Los sorteos durante la ejecución usan BLAKE2b de `article7-v02|semilla|flujo|mes|claves`. Se toman 53 bits uniformes. La política no forma parte de la clave. Los flujos del código distinguen orden de solicitudes (10), respuesta (11), targeting (12), ofertas (13), activación (14), destinatario (15), orden de exposición (20), necesidad (21), exposición decisiva (22), compra (23), capacidad (24), invitación (30), límite de invitados (31), asistencia (32), aforo (33) y enlaces (34).
4. La necesidad mensual se evalúa mediante el mismo sorteo indexado cuando la persona recibe exposición. Esta evaluación diferida equivale a sortearla al inicio y no modifica el resultado de nadie sin exposición.
5. La exposición decisiva es la de menor prioridad uniforme entre las exposiciones del receptor. Esto elige uniformemente una exposición y permite emparejar las comunes entre políticas. Una recomendación bajo incentivo utiliza la misma clave estructural de una orgánica del mismo emisor, receptor y mes.
6. La energía es un recurso del cliente; promotor elegible y cliente satisfecho no son categorías equivalentes. Un cliente satisfecho puede responder a una solicitud aunque no tenga disposición a emitir WOM espontáneo. El indicador de actividad considera únicamente las recomendaciones espontáneas orgánicas o bajo incentivo, conforme a v0.1.
7. Cada evento reserva por anticipado el costo de aforo máximo y las horas máximas de celebración. En el mes de celebración esas horas se descuentan de la disponibilidad para otras campañas antes de ejecutar acciones. Al final se cobran los asistentes reales y se libera el remanente.
8. Las proporciones 40/30/30 de la mezcla limitan tanto dinero como tiempo por canal. Los saldos no se transfieren; la secuencia y la adaptación sí usan el presupuesto común.
9. Una solicitud fallida conserva costo y horas. Su registro se exporta, además del registro de las recomendaciones emitidas. Las ofertas no convertidas liberan toda su reserva y mantienen su costo administrativo de tiempo.
10. Los CSV usan `NA` para valores indefinidos, `True/False` para booleanos y JSON dentro de celdas para listas/diccionarios. Cada ejecución tiene `run_id=s{semilla}_{política}`. Los datos iniciales compartidos se exportan una sola vez por semilla y el manifiesto enlaza cada ejecución con esa condición.
11. El archivo de configuración contiene las constantes de decisión, presupuesto y comportamiento. El código rechaza configuraciones inválidas y directorios de salida no vacíos. No sobrescribe ni mezcla experimentos automáticamente.
12. La regla adaptativa recibe indicadores observables y factibilidad, sin acceder a ajuste al producto, necesidad, energía real o red completa. La factibilidad incluye recursos y posibilidad de organizar invitaciones. Se presupone reporte completo de interacciones, como en v0.1.

## Alcance de la verificación y del experimento

Las pruebas verifican programación y coherencia interna. No son validación empírica ni demuestran que los supuestos representan una empresa real. El piloto usa semillas 1001–1010. El escenario principal usa 2001–2100. Las repeticiones se emparejan entre políticas y ambas muestras permanecen separadas.

Antes del experimento principal se revisan las 70 ejecuciones piloto. Los resultados inesperados se conservan. En particular, el modelo puede producir crecimiento lento, costos superiores al margen, ausencia de demanda no atendida y escasa utilización de algunos componentes de la política adaptativa. Estos patrones deben orientar la siguiente fase de sensibilidad, sin reinterpretarse como errores por sí mismos.

La comparación basal no completa los experimentos de presupuesto, modularidad, capacidad, timing e intensidad necesarios para el manuscrito. No permite construir todavía recomendaciones óptimas por estado de red. El resultado entregado es una línea de base reproducible para esos experimentos.

## Orden de implementación documentado

Red y personas → WOM y compra → referidos → incentivos y reservas → eventos y enlaces → políticas → auditoría → piloto → comparación basal. La carpeta de modelo pasa de `01_Modelo_NetLogo` a `01_Modelo_Python`.
