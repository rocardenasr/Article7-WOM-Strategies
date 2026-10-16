# Article 7 — Verificación de órdenes de reglas v0.4

Se superaron **39 pruebas** sin fallos: 24 del motor, cinco de sensibilidad y diez de orden. El registro íntegro es `docs/rule_order_verification_v0.4.txt`.

Las **2.400 ejecuciones** tienen auditoría `passed`: 144.000 periodos mensuales. Incluyen 2.300 casos alternativos y 100 reproducciones del original. Las reproducciones coinciden con 22 campos del resumen por semilla —además de la clave seed— y con las 6.000 filas completas de decisiones archivadas. No se cuenta la referencia dos veces en la inferencia.

| Control | Resultado |
|---|---|
| Enumeración | 24 órdenes únicos de 4, 5, 6 y 7 |
| Selector original en fronteras | 2.000 estados comprobados contra la implementación original |
| Trayectorias originales completas | Igualdad de todos los registros en semillas 2001, 2002 y 2003 |
| Guardas y pausa final | Prioridad original preservada para las 24 permutaciones |
| Condiciones simultáneas e inviabilidad | Selección correcta de la primera acción elegible y factible |
| Políticas fijas y campañas desactivadas | Igualdad con el motor original en pruebas específicas |
| Auditoría de decisiones | Reconstrucción con oráculo independiente; detecta una decisión adulterada |
| Contabilidad y trazas | Todas las ejecuciones superan las reconciliaciones originales |
| Comparación emparejada | 100 casos únicos por variante y las mismas semillas que el basal |
| Series observadas | 12 grupos de archivos mensuales idénticos entre las 24 permutaciones |

El oráculo de auditoría no llama al selector ni a las condiciones de producción. Se reutiliza el cuerpo del auditor contable original con un espacio de nombres aislado; los módulos originales conservan código y comportamiento. Las guardas no forman parte de las permutaciones.

Diseño SHA-256: `333e57b0af2767be61a8d5bbf163cdd8ad6fc069d026fbde5eb0505bb527fd5d`. Los hashes del motor, ejecutor, subclase y nuevo ejecutor aparecen en cada `experiment_manifest.json`. El hash del analizador figura en `analysis_manifest.json`.

`release_verification.json`, dentro del paquete de análisis, registra las comprobaciones de entrega: integridad CRC de ZIP, hashes de fuentes del paquete extraído, ejecución de las 39 pruebas desde esa extracción, sintaxis del notebook y reproducción de una variante contra el archivo experimental. Las cifras de pruebas se refieren a casos de prueba, no a simulaciones adicionales del experimento.

Los dos gráficos se revisaron visualmente. Los intervalos marginales y simultáneos se contrastaron con los resúmenes emparejados; una identidad observada no se interpreta como equivalencia global. La verificación computacional no demuestra calibración ni validez empírica. Colab no se probó a través de su interfaz; sí se ejecutaron los comandos Python y se comprobó la sintaxis de sus celdas.
