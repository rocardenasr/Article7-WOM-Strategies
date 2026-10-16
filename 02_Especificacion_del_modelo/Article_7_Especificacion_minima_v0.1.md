# Article 7 — Especificación mínima v0.1

**Título provisional:** Referrals, Rewards, or Events? An Agent-Based Model of Adaptive Entrepreneurial Word-of-Mouth Strategy  
**Proyecto:** The 22 Manuscripts – Season II · Article 7 · Roiman  
**Coordinación:** Universidad Nacional de Colombia, Sede de La Paz  
**Fecha:** 25 de septiembre de 2026  
**Estado:** propuesta operativa para implementar y verificar; pendiente de revisión conceptual del equipo.  
**Documento base:** `Article 7 - NM - 4 -Strategies.md`, guía de ejecución suministrada por el profesor.  
**Carpeta del proyecto:** https://drive.google.com/drive/folders/1tR2VQlCjeWagnKoW7BXpE-4xt2bPc695

## 1. Propósito y pregunta de investigación

El estudio analizará cómo un emprendedor puede seleccionar y secuenciar referidos, incentivos y eventos a medida que evoluciona su red de clientes, bajo restricciones de presupuesto, tiempo y capacidad de servicio.

**Pregunta central:** ¿bajo qué condiciones y en qué momentos resulta más útil cada intervención de boca a boca —word of mouth, WOM— y qué diferencias produce una política adaptativa frente a políticas fijas, mixtas y secuenciales?

El aporte consiste en examinar mecanismos, costos, secuencias y condiciones de utilidad. No se supondrá que una intervención ni la política adaptativa debe ganar. Los referidos se representarán como un mecanismo de selección y confianza; los incentivos, como un mecanismo de activación de promotores; y los eventos, como un mecanismo de exposición y conectividad.

Este es un ejercicio teórico y computacional. No habrá calibración con datos de empresas, encuestas, entrevistas o campañas. Todos los valores numéricos siguientes son **supuestos ilustrativos y revisables**, incluidos los que provienen de la guía. No constituyen estimaciones empíricas ni recomendaciones universales.

## 2. Alcance de la versión inicial

La implementación principal utilizará NetLogo; los resultados se analizarán en Python o R. Antes de ejecutar experimentos se registrarán las versiones exactas utilizadas. Este documento no contiene código ejecutado ni resultados de simulación.

| Decisión | Definición v0.1 |
|---|---|
| Unidad organizacional | Una empresa, un producto y un precio constante. |
| Mercado | 600 personas en total, incluidos 15 clientes iniciales; quedan 585 posibles nuevas adquisiciones. |
| Red | Seis comunidades de 100 personas, enlaces no dirigidos y sin enlaces duplicados ni autorrelaciones. |
| Tiempo | Un paso equivale a un mes; horizonte de 60 meses. |
| Compra | Una compra por persona durante el horizonte. No hay recompra ni abandono de clientes. |
| Promotores | Clientes satisfechos con disposición a recomendar; la energía determina su actividad. |
| Restricciones | Presupuesto monetario de campañas, horas de marketing y capacidad mensual de servicio. |
| Políticas | Orgánica, referidos, incentivos, eventos, mezcla simultánea, secuencia fija y adaptativa. |
| Dinámica de red | La red inicial permanece fija; únicamente los eventos pueden añadir enlaces. |
| Fuera de alcance | Competidores, MLM, capacidades empresariales variables, tipos de influencers, aprendizaje por refuerzo y WOM negativo. |

Se simplifica la guía al excluir recompra, reinversión automática y deterioro de satisfacción por espera. La capacidad se incluye desde el inicio. Estas decisiones permiten verificar primero la adquisición y la comparación de políticas; su consecuencia es que los resultados describirán la difusión de una compra única en un mercado finito.

## 3. Entidades, estados e inicialización

### 3.1 Personas y empresa

Cada persona tendrá: identificador, comunidad, ajuste al producto `fit`, conocimiento de la oferta, número de exposiciones, condición de cliente, satisfacción, disposición a recomendar, energía, mes de compra, fuente de adquisición y contactos ya referidos.

Se separará el estado comercial de la condición de promotor para evitar categorías incompatibles. Una persona puede ser cliente y promotor al mismo tiempo.

- **No informada:** no conoce la oferta.
- **Informada:** conoce la oferta, pero aún no compra.
- **Prospecto del mes:** recibe una exposición válida y tiene necesidad de compra ese mes.
- **Demanda del mes:** prospecto cuya decisión de compra resulta positiva.
- **Cliente:** compra atendida; condición permanente en v0.1.
- **Demanda no atendida:** resultado mensual cuando una decisión de compra no puede servirse; la persona sigue informada y no se convierte en cliente.

Un cliente satisfecho y dispuesto a recomendar es un **promotor elegible**. Es **activo en el mes** si emite al menos una recomendación orgánica o bajo incentivo. La energía será una variable continua entre 0 y 1; la inactividad mensual no elimina la elegibilidad.

La empresa registrará presupuesto libre y comprometido, caja, horas consumidas, capacidad, ventas, costos, utilidades, política, acción seleccionada, compromisos de eventos y motivos de decisión.

### 3.2 Red inicial

Se generará una red por bloques: para cada par de personas se sortea un enlace con probabilidad `p_in` si pertenecen a la misma comunidad y `p_out` si pertenecen a comunidades diferentes. Valores iniciales: `p_in = 0.08` y `p_out = 0.004`.

No se añadirán enlaces para forzar conectividad. Se registrarán grado medio, componentes, tamaño del mayor componente, clustering, modularidad de la partición conocida y proporción de enlaces entre comunidades. La modularidad observada será un resultado diagnóstico; no se confundirá con una probabilidad de enlace.

Los 15 clientes iniciales se seleccionarán aleatoriamente en la comunidad 1, entre personas con `fit ≥ 0.60`. Si una realización no contiene 15 candidatos, se regenerarán únicamente los atributos de ajuste de esa comunidad mediante una regla registrada. Los clientes iniciales estarán informados y satisfechos; seis serán promotores elegibles, seleccionados al azar. Todos tendrán energía inicial igual a 1. Sus compras previas no generarán ingresos ni utilidad dentro del experimento. Las otras 585 personas empezarán sin conocer la oferta; no habrá exposiciones previas ni eventos pendientes.

Para las demás personas, `fit ~ Uniforme(0.20, 1.00)` y la disposición a recomendar será Bernoulli con probabilidad 0.40, fijada antes de comparar políticas. La satisfacción de cada futuro comprador se sorteará con probabilidad 0.80 y se conservará como atributo potencial común entre políticas. Al comprar tendrá energía 1 y podrá recomendar desde el mes siguiente.

### 3.3 Comparaciones emparejadas

Cada repetición compartirá entre las siete políticas la misma red inicial, atributos, clientes y promotores iniciales. Necesidad, satisfacción, activación, selección de contactos y conversión utilizarán números aleatorios reproducibles indexados por repetición, mes, persona y mecanismo. No basta con reiniciar una sola semilla si las ramas de las políticas consumen diferentes cantidades de números aleatorios.

Se implementarán tablas de sorteos o flujos separados y se verificará su equivalencia antes del piloto. No se usará un hash dependiente de la sesión del lenguaje. La divergencia posterior de las redes por eventos será parte del efecto de la política.

## 4. Reglas comunes de exposición, compra y servicio

1. Al inicio de cada mes, cada no cliente tendrá necesidad con probabilidad 0.30. Este sorteo será común entre políticas y no se modificará por recibir incentivos o asistir a eventos.
2. Una exposición comunica la oferta a una persona. La primera cambia su conocimiento; las posteriores se registran como repetidas. Conocer la oferta no garantiza comprar.
3. Para entrar al proceso de compra, la persona debe tener necesidad y recibir al menos una exposición válida ese mes. No se modelará compra espontánea por recuerdo sin una nueva exposición.
4. Si recibe varias exposiciones, se elegirá una uniformemente mediante un sorteo reproducible para representar la conversación decisiva del mes. Esa exposición determina la probabilidad de conversión y la atribución; las demás se conservan en el registro. No se sumarán probabilidades ni se realizarán varias compras.
5. Probabilidad de decisión de compra: `p_compra = min(1, fit × (0.25 + bono_referido))`. El bono vale 0.15 únicamente para una exposición decisiva de referido solicitado; vale cero para WOM orgánico, WOM bajo incentivo y eventos.
6. Si las decisiones positivas superan la capacidad, se atenderá una muestra aleatoria uniforme, sin prioridad por canal. Solo las compras atendidas generan ingresos, clientes y recompensas.
7. La demanda no atendida se pierde en ese mes: no hay cola automática. La persona puede volver a intentar comprar en otro mes si nuevamente tiene necesidad y recibe exposición. Se reportarán tanto oportunidades perdidas como personas únicas afectadas.

Los eventos no tienen un bono de conversión en esta versión. Esto permite examinar sus efectos de exposición y conectividad sin añadir simultáneamente otra ventaja de confianza. La atribución a un canal es una convención contable; no demuestra el efecto causal aislado de una intervención con interacciones acumuladas.

## 5. Mecanismos de WOM e intervenciones

### 5.1 WOM orgánico y energía

Todo promotor elegible con energía suficiente puede emitir como máximo una recomendación espontánea por mes. Selecciona uniformemente un vecino que aún no sea cliente. El destinatario puede haber recibido exposiciones previas.

`p_orgánico = 0.25 × energía`.

Cada recomendación emitida consume 0.10 de energía, aunque no produzca compra. Se requiere energía de al menos 0.10. Al cierre del mes todos los clientes recuperan 0.05, con máximo de 1. La satisfacción y la disposición a recomendar permanecen constantes.

El WOM orgánico opera en las siete políticas y continúa cuando no hay campañas o se agota el presupuesto. No consume horas de marketing de la empresa; sí consume energía del emisor. Los nuevos clientes no propagan WOM dentro del mismo mes en que compran.

### 5.2 Referidos solicitados

La empresa podrá solicitar hasta 12 referidos por mes. Los clientes satisfechos se ordenarán aleatoriamente, con máximo de una solicitud por cliente y mes. Cada solicitud consume 1 unidad monetaria —UM— y 0.50 horas, incluso si no obtiene respuesta útil.

Un cliente acepta recomendar con probabilidad 0.60 si tiene energía suficiente. Selecciona un vecino no cliente al que él mismo no haya referido antes. Para representar identificación imperfecta de prospectos relevantes, se asignará a cada candidato peso `1 + theta_fit × fit`, con `theta_fit = 1`, y se sorteará proporcionalmente a ese peso. La empresa no usa `fit` para elegir la política. No se filtran perfectamente las necesidades ni se garantiza conversión. Si no hay candidatos, la solicitud se registra sin referido y conserva su costo; no consume energía de transmisión.

Si se emite el referido, consume 0.10 de energía y se registra emisor, receptor, exposición y solicitud. El receptor recibe el bono de confianza definido en la sección 4 si ese referido es su exposición decisiva. No se descarta un receptor solo por haber sido expuesto por otra persona: así puede emerger redundancia.

Los referidos se procesarán antes del WOM espontáneo. Un cliente puede emitir un referido y una recomendación espontánea en el mes si cumple las condiciones y conserva energía. El mismo par dirigido no admite un segundo referido solicitado durante la ejecución; las recomendaciones espontáneas sí pueden repetirse.

### 5.3 Incentivos

La empresa seleccionará aleatoriamente hasta 20 promotores elegibles por mes para ofrecer una recompensa `r = 2 UM`. Se requerirán 0.10 horas administrativas y una reserva monetaria de `r` por oferta, antes de modificar la conducta. Solo se ofrecerán recompensas que puedan pagarse.

Para un promotor con oferta, la probabilidad de recomendar será:

`p_incentivo = min(1, (0.25 + 0.50 × r / (r + 2)) × energía)`.

Se utilizará el mismo sorteo uniforme `u` de la recomendación orgánica: si `u < p_orgánico`, la activación es basal; si `p_orgánico ≤ u < p_incentivo`, la activación es adicional por la oferta. El destinatario se seleccionará con la misma regla orgánica. El promotor podrá emitir una sola recomendación espontánea bajo esta regla, no una orgánica adicional más otra incentivada.

La recompensa se pagará únicamente si esa recomendación es la exposición decisiva de una compra atendida en el mismo mes. Se pagará también cuando la activación fue basal: esto representa recompensar una recomendación que habría ocurrido sin el aumento de activación, condicionado al estado de ese mes. Se registrarán por separado `oferta`, `activación_basal`, `activación_adicional` y `pago`.

Al recibir el pago, el promotor recuperará `0.20 × r / (r + 2)` de energía, además de la recuperación ordinaria, sin superar 1. La reserva se liberará si no hay compra atendida. No hay obligaciones de pago en meses posteriores. Para `r = 0`, la intervención se desactiva: no aumenta actividad ni energía y no consume dinero ni administración.

La etiqueta de activación adicional describe un mecanismo local del código. Para estimar diferencias acumuladas frente a no ofrecer incentivos se usarán comparaciones emparejadas; no se interpretará esa etiqueta como el efecto causal total sobre las ventas.

### 5.4 Eventos

Los eventos requieren preparación de un mes. Al planificar se pagarán 60 UM, se consumirán 8 horas y se reservarán 30 UM para asistencia y 5 horas del mes siguiente. Se podrán celebrar como máximo 30 asistentes. El costo variable será 1 UM por asistente y el tiempo de celebración `2 + 0.10 × asistentes` horas. El costo fijo de preparación no se devuelve si nadie asiste.

La lista de invitación se formará con los clientes existentes y un vecino no cliente elegido uniformemente por cada cliente que tenga alguno. Se eliminarán duplicados y, si hay más de 60 personas, se sortearán 60 invitaciones. Las invitaciones no cuentan como exposición a la oferta. La lista queda fijada al planificar; la asistencia se sortea al celebrar, con probabilidad 0.60 por invitado. Si más de 30 aceptan, se sortearán 30 asistentes. Los promotores y clientes pueden asistir, pero no volver a comprar.

Cada asistente no cliente recibe una exposición. Con al menos seis asistentes, cada par sin enlace puede crear uno nuevo con probabilidad 0.10 si pertenece a comunidades distintas y 0.02 si pertenece a la misma. Los enlaces se añaden al final del mes y podrán transmitir WOM desde el siguiente. No hay efecto de reactivación de energía por eventos en v0.1.

Toda conexión nueva debe unir asistentes reales. Cero asistentes implica cero exposiciones y cero enlaces. Con menos de seis asistentes puede haber exposición y compra, pero no enlaces nuevos. Se reportarán asistencia, diversidad, vínculos creados, cruces de comunidades y resultados comerciales por separado.

No habrá invitados externos a la red accesible de los clientes. Por tanto, los eventos pueden crear atajos y puentes entre comunidades representadas, pero no alcanzar por sí solos un componente completamente desconectado de todos los clientes iniciales. Esta es una condición de frontera explícita.

Se permitirá como máximo un evento pendiente y seis meses entre inicios de preparación. No se iniciará preparación en el mes 60. Los compromisos asumidos se cumplirán el mes siguiente, aunque después aumente la ocupación de capacidad. Tras celebrar se liberará el dinero reservado para plazas no ocupadas. La política adaptativa no podrá cancelar costos ya asumidos para mejorar artificialmente su resultado.

## 6. Recursos y contabilidad

Todas las políticas reciben `B0 = 1,200 UM`, `T = 40 horas de marketing al mes` y capacidad `K = 20 compras atendidas al mes`. Las horas no usadas caducan al cierre; el presupuesto no gastado permanece. La capacidad representa una operación de servicio separada; no se trasladan horas de marketing a producción ni viceversa. El costo variable del servicio incluye su trabajo operativo.

El presupuesto de campañas será cerrado, sin crédito ni reinversión automática de ingresos. La caja se calculará por separado. Esta decisión evita que una ventaja temprana modifique también el presupuesto disponible para la comparación principal. La reinversión podrá estudiarse después como escenario adicional.

Antes de iniciar una acción se comprobarán dinero libre, compromisos vigentes y horas del mes. Los eventos también comprobarán las horas comprometidas del mes siguiente. Una reserva no es un gasto ni utilidad negativa hasta ejecutarse el pago correspondiente.

Identidades obligatorias, con valores acumulados cuando corresponda:

- `B0 = gasto_marketing + reservas_pendientes + presupuesto_libre`.
- `ingresos = 20 × compras_atendidas`.
- `costo_servicio = 12 × compras_atendidas`.
- `utilidad_operativa = ingresos − costo_servicio − costo_referidos − recompensas − costo_eventos`.
- `costo_oportunidad = 2 × horas_marketing_realmente_utilizadas`.
- `utilidad_ajustada = utilidad_operativa − costo_oportunidad`.
- `caja = B0 + utilidad_operativa_acumulada`.

La dotación inicial no es ingreso ni utilidad. Se reportarán utilidad operativa y utilidad ajustada para distinguir desembolsos y costo de tiempo. Un evento preparado y aún no celebrado ya habrá generado el costo fijo. Los costos no se imputarán dos veces al agregar intervenciones.

## 7. Políticas a comparar

En todas existe WOM orgánico. Las políticas comparten reglas de servicio, probabilidades básicas y límites máximos de cada intervención.

| Código | Política | Regla operativa |
|---|---|---|
| A | Orgánica | No ejecuta campañas. |
| B | Referidos | Intenta hasta 12 solicitudes cada mes, dentro de recursos disponibles. |
| C | Incentivos | Ofrece recompensas a hasta 20 promotores cada mes, dentro de recursos disponibles. |
| D | Eventos | Intenta preparar eventos en meses 1, 7, 13, …, 55; celebra al mes siguiente. |
| E | Mezcla simultánea | Asigna 40% del presupuesto inicial y de las horas mensuales a referidos, 30% a incentivos y 30% a eventos. Mantiene el calendario de D. |
| F | Secuencia fija | Referidos en meses 1–20; incentivos en 21–40; prepara eventos en 41, 47, 53 y 59. |
| G | Adaptativa | Selecciona una nueva intervención o una pausa cada mes según la sección 8. |

En E, los subpresupuestos monetarios conservan sus saldos sin transferencias; las horas se renuevan mensualmente y no se transfieren. Así, los límites suman exactamente 1,200 UM y 40 horas. Una acción que no cabe en su parte no se ejecuta. El fraccionamiento y los recursos inmovilizados son parte de esta política, no una característica universal de cualquier mezcla.

En D y E, si la preparación programada no es factible por recursos o porque no hay invitados, se registra su omisión y se espera al siguiente mes programado. En F, el dinero no gastado pasa a la siguiente etapa. Las políticas fijas no se suspenden por alta ocupación: su estímulo puede generar demanda no atendida. Ninguna podrá superar capacidad ni recursos.

## 8. Política adaptativa observable

La empresa dispone de un registro de contactos con identidad, comunidad declarada, exposiciones registradas, recomendaciones reportadas y compras. Se supone reporte completo y sin error de las interacciones generadas por el modelo; es una simplificación favorable a la observación, común a las políticas. No observa ajuste al producto, necesidad latente, energía individual real, todos los enlaces ni personas nunca contactadas. Esas variables se reservan para el análisis del investigador.

Indicadores calculados con información disponible al cierre del mes anterior:

- `C`: clientes acumulados y `P`: promotores elegibles inscritos.
- `U3`: compras atendidas en los últimos hasta tres meses / capacidad disponible en esos meses. Si la capacidad total del período es cero, vale `NA` y se activa la pausa por capacidad nula.
- `A3`: suma de promotores activos por mes / suma de promotores elegibles al inicio de cada mes, en esa ventana. Denominador cero produce `NA`.
- `R3`: exposiciones repetidas / exposiciones totales de esa ventana. El conocimiento previo incluye a los clientes iniciales. Denominador cero produce `NA`.
- `M3 = 1 − R3`: proporción de exposiciones que alcanzan por primera vez a una persona.
- `Gobs`: comunidades distintas identificadas entre clientes y personas contactadas.

Las reglas se evalúan en el siguiente orden; gana la primera que se cumpla:

| Prioridad | Condición | Decisión |
|---|---|---|
| 1 | Hay evento comprometido para este mes | Celebrarlo; no iniciar otra campaña adaptativa ese mes. |
| 2 | Capacidad cero o `U3 ≥ 0.90` | Pausar campañas. |
| 3 | No hay dinero/tiempo suficiente para ninguna intervención o el mercado conocido como cliente alcanza 600 | Pausar campañas. |
| 4 | `C < 30` o `P < 10` | Referidos, si son factibles. |
| 5 | Al menos 20 exposiciones en la ventana y (`R3 ≥ 0.60` o `Gobs ≤ 2`) | Preparar evento, si cumple presupuesto, tiempo, calendario y tiene al menos 10 invitados. |
| 6 | Al menos tres meses observados, `A3 < 0.20`, al menos 20 exposiciones y `R3 < 0.60` | Incentivos, si son factibles. |
| 7 | Menos de tres meses observados, o `M3 ≥ 0.25` con al menos 20 exposiciones | Referidos, si son factibles. |
| 8 | Ninguna anterior produce una acción factible | Pausar campañas. |

Si una regla selecciona una acción no factible, se registra el motivo y se continúa con la siguiente prioridad; las pausas de las prioridades 2 y 3 terminan la decisión. Las reglas no usan el estado verdadero de saturación ni anticipan ventas o asistentes. En el mes 1 solo se dispone de condiciones iniciales; los indicadores históricos son `NA`.

Los umbrales son una propuesta transparente para probar, no valores óptimos. Se examinarán sensibilidad, prioridad de reglas y alternativas de secuencia después del piloto. No se ajustarán utilizando los resultados del experimento principal para favorecer G.

## 9. Orden mensual de ejecución

1. Cargar estado inicial del mes, compromisos previos y sorteos comunes; actualizar necesidad de los no clientes.
2. Calcular indicadores con datos hasta el mes anterior y seleccionar política/acción; registrar condiciones y motivos.
3. Verificar y reservar recursos; preparar eventos futuros y fijar sus listas de invitados.
4. Ejecutar solicitudes de referidos y descontar costos, tiempo y energía cuando corresponda.
5. Seleccionar ofertas de incentivos; generar WOM espontáneo orgánico o bajo oferta con los sorteos comunes.
6. Celebrar eventos pendientes y registrar asistencia y exposiciones.
7. Procesar todas las exposiciones en un orden aleatorio común por mes, mecanismo, emisor y receptor; actualizar conocimiento y repetición.
8. Elegir exposición decisiva por receptor; sortear compra y atender bajo capacidad. Registrar demanda no atendida.
9. Pagar recompensas atribuibles, liberar reservas e incorporar nuevos clientes. Sus acciones empiezan el mes siguiente.
10. Recuperar energía, añadir enlaces de eventos, cerrar contabilidad y exportar datos.

Las identidades, fechas y reglas de desempate quedarán registradas. Se conservará este orden en todas las políticas; modificarlo requerirá nueva versión de la especificación.

## 10. Tabla de parámetros iniciales

Los valores de esta tabla y las constantes de las secciones anteriores deberán aparecer en un archivo de configuración exportado; no quedarán ocultos en el código.

| Grupo | Parámetros v0.1 | Sensibilidad inicial propuesta |
|---|---|---|
| Mercado y tiempo | 600 personas; 6 comunidades; 60 meses | 300/1,200 personas como robustez posterior, preservando proporciones y documentando escala de recursos. |
| Inicio | 15 clientes; 6 promotores; comunidad 1 | 10/20 clientes y distribución en 1/2 comunidades. |
| Red | `p_in=0.08`; `p_out=0.004` | Mayor/menor segregación conservando grado esperado. |
| Demanda | Necesidad 0.30; `fit ~ U(0.20,1.00)` | Necesidad 0.15/0.45. |
| Compra y satisfacción | Conversión basal 0.25 × fit; satisfacción 0.80; disposición 0.40 | Conversión basal 0.15/0.35; satisfacción 0.60/0.95. |
| Recursos | Presupuesto 1,200 UM; 40 h/mes; capacidad 20/mes | Presupuesto 600/2,400; horas 20/80; capacidad 5/40. |
| Economía | Precio 20; costo servicio 12; costo oportunidad 2 UM/h | Costo oportunidad 0/4; costos de intervención ×0.5/×2. |
| Energía | Inicial/máxima 1; recuperación 0.05; consumo 0.10 | Recuperación 0/0.10. |
| WOM | Coeficiente orgánico 0.25; máximo 1 recomendación espontánea/mes | Coeficiente 0.10/0.50. |
| Referidos | 12 solicitudes; respuesta 0.60; 1 UM y 0.50 h/solicitud; bono 0.15; theta_fit=1 | Bono 0/0.30; theta_fit=0 como ablación del targeting. |
| Incentivos | 20 ofertas; recompensa 2; 0.10 h/oferta; aumento máximo 0.50; semisaturación 2; recuperación máxima 0.20 | Recompensa 0/1/4; energía por recompensa desactivada. |
| Eventos | 60 UM fijos; 1 UM/asistente; 60 invitaciones; aforo 30; asistencia 0.60; mínimo 6 para enlaces | Asistencia 0.30/0.90; efectos de enlaces desactivados. |
| Eventos: vínculos y tiempo | Probabilidad entre comunidades 0.10; dentro 0.02; preparación 8 h; celebración 2+0.10×asistentes h | Igualar probabilidades entre/dentro; variar costo fijo. |
| Decisión | Ventana 3 meses; mínimo 20 exposiciones; C=30; P=10; ocupación 0.90; redundancia 0.60; actividad 0.20; alcance marginal 0.25; Gobs≤2; 10 invitados | Umbrales: variaciones de ±20%, acotadas a dominios válidos; calendario de 3/9 meses. |

Para variar estructura sin confundirla con densidad: grado esperado basal `k = 99×0.08 + 500×0.004 = 9.92`. Usar `p_out=0.001` con `p_in=(9.92−500×0.001)/99≈0.09515`, y `p_out=0.010` con `p_in≈0.04970`. La densidad realizada puede variar; se registrará. Las redes se emparejan entre políticas dentro de cada condición.

## 11. Resultados y archivos de salida

Las medidas principales serán nuevas adquisiciones acumuladas y utilidad ajustada al mes 60. Se reportará también su trayectoria para no ocultar diferencias de timing. Habrá ventas tempranas al mes 12, utilidad operativa, promotores activos, alcance, redundancia, demanda no atendida y recursos utilizados.

La saturación verdadera será personas informadas / personas del mercado, globalmente y por comunidad; se calcula para análisis, no se entrega al decisor. Comunidades con exposición y comunidades con clientes se reportarán por separado. El costo por nuevo cliente será gasto de campañas / nuevas adquisiciones; denominador cero produce `NA`, no cero. No se comparará el costo orgánico de cero como si demostrara mayor efectividad comercial.

| Archivo propuesto | Unidad y contenido mínimo |
|---|---|
| `run_manifest.csv` | Ejecución: run_id, política, escenario, repetición, semillas, versión de código/configuración, software, estado y fecha. |
| `period_results.csv` | Ejecución–mes: clientes, promotores, exposiciones nuevas/repetidas, demanda, ventas atendidas/no atendidas, ingresos, costos desagregados, utilidades, presupuesto/reservas, horas, alcance y red. |
| `recommendations.csv` | Transmisión: emisor, receptor, mes, tipo, oferta de incentivo, activación basal/adicional, energía antes/después y exposición asociada. |
| `exposures_and_sales.csv` | Exposición, incluida la de eventos: receptor, fuente, mes, exposición previa, indicador decisivo, necesidad, decisión de compra, atención, atribución y pago. |
| `events.csv` | Evento: preparación/celebración, lista de invitados/asistentes o archivo relacionado, diversidad, exposición, enlaces, costos y horas. |
| `decisions.csv` | Decisión: indicadores disponibles, umbrales, regla elegida, acciones descartadas, factibilidad, compromiso y motivo. |
| `run_summary.csv` | Ejecución: indicadores finales, ventas tempranas, costos, utilidad, pausas y cambios de estrategia. |
| `initial_network.csv` / `event_links.csv` | Pares de nodos, comunidades, ejecución y procedencia; para enlaces nuevos, evento y mes. |
| `parameters.csv` / `agents_initial.csv` | Configuración completa y atributos iniciales necesarios para reproducir la ejecución. |

Todos los registros incorporarán `run_id`; los datos de eventos y exposiciones tendrán identificadores que permitan verificar su ascendencia. `NA` se reservará para medidas indefinidas. Los errores y ejecuciones interrumpidas se conservarán con su estado, sin reemplazarlos silenciosamente.

## 12. Pruebas obligatorias antes del piloto

| ID | Condición | Resultado que debe comprobarse |
|---|---|---|
| V01 | Cero clientes iniciales | No hay referidos, promotores ni invitados por los mecanismos internos definidos; no aparecen ventas espontáneas. |
| V02 | Incentivo cero | Igual actividad que sin oferta, sin pago, administración ni recuperación adicional. |
| V03 | Evento con cero asistentes | Cero exposiciones, enlaces y ventas atribuibles; se conserva el costo fijo ya pagado. |
| V04 | Comunidades desconectadas; todos los clientes en una | No aparecen clientes en otros componentes por WOM, referidos o eventos internos. |
| V05 | Presupuesto insuficiente o cero | No se aceptan compromisos que excedan recursos; WOM orgánico continúa. |
| V06 | Horas insuficientes o cero | No se inicia una acción sin tiempo; los eventos reservan el mes siguiente. |
| V07 | Capacidad cero o demanda superior a capacidad | Ventas atendidas ≤ K; ingresos y recompensas solo sobre ventas atendidas. |
| V08 | Referido y venta atribuida | Emisor es cliente satisfecho previo; receptor es vecino y no cliente al emitir; existe trazabilidad. |
| V09 | Enlaces de evento | Ambos extremos asistieron; no hay autorrelaciones, duplicados ni enlaces antes de su fecha. |
| V10 | Adaptación | Cada decisión se reconstruye exclusivamente desde datos disponibles y reglas de prioridad. |
| V11 | Todos los efectos de intervención desactivados | Con sorteos comunes, exposición efectiva, adopción y red coinciden con A; costos hipotéticos se evalúan por separado, sin alterar energía ni conducta. |
| V12 | Políticas equivalentes con iguales costos y mecanismos | Resultados idénticos bajo condiciones emparejadas, no solo medias parecidas. |
| V13 | Contabilidad | Se cumplen las identidades de presupuesto, caja, ingresos y ambas utilidades; sin reservas duplicadas. |
| V14 | Misma configuración y semillas | Se repiten exactamente los resultados y registros, salvo marcas de tiempo. |
| V15 | Exposiciones múltiples y capacidad escasa | Una compra por persona; máximo un pago por adquisición; atribución única y no dependiente del orden de las políticas. |
| V16 | Agotamiento y recuperación | Energía entre 0 y 1; nadie transmite sin energía suficiente; nuevos clientes esperan al siguiente mes. |
| V17 | Mezcla y secuencia | Subrecursos suman el total; los cambios ocurren en meses previstos; compromisos no desaparecen. |
| V18 | Límites de indicadores | Divisiones por cero producen `NA`; se cumplen ventanas y mínimos de observaciones. |
| V19 | Horizonte y recursos reservados | No hay nuevos eventos en mes 60; al cierre no quedan eventos ni recompensas pendientes. |
| V20 | Registro y agregación | La suma de registros individuales coincide con series mensuales y resúmenes. |

Se añadirán pruebas con una red manual pequeña para seguir personas y pagos paso a paso. La ablación de V11 desactiva de forma explícita exposiciones, targeting, confianza, cambios de energía y enlaces adicionales; desactivar solo el bono de conversión no elimina todos los efectos de una intervención. En una contabilidad de costos superpuesta al mismo recorrido comercial, la política de menor costo debe tener mayor o igual utilidad.

## 13. Piloto, experimento principal y sensibilidad

**Piloto propuesto:** 10 repeticiones emparejadas × 7 políticas = **70 ejecuciones**, con 600 personas y 60 meses. Identificadores de semilla del piloto: 1001–1010. Su finalidad será revisar estabilidad, duración, trazabilidad, costos, difusión y utilidad de los indicadores. No será evidencia suficiente para declarar superioridad de una política.

Se pasará al experimento principal cuando todas las pruebas obligatorias aprueben, las ejecuciones terminen sin violaciones de recursos y se pueda reproducir una trayectoria completa. Los resultados inesperados no serán motivo de rechazo por sí solos. Si ninguna intervención llega a activarse o los indicadores no distinguen estados, se revisará la especificación y se explicará por qué; no se cambiarán parámetros solo para obtener el orden esperado.

**Experimento principal inicial:** 100 repeticiones emparejadas × 7 políticas = **700 ejecuciones** en el escenario basal. Identificadores de semilla: 2001–2100, distintos del piloto. La generación de subsemillas deberá quedar documentada y verificada. Las 70 ejecuciones del piloto no se sumarán a la muestra principal.

**Sensibilidad prioritaria:** presupuesto, estructura de red manteniendo grado esperado, capacidad, intensidad de incentivos, costos y umbrales adaptativos. Se preparará una matriz de condiciones antes de ejecutarla; no se cruzarán automáticamente todos los parámetros. Como exploración inicial se podrán usar 30 repeticiones por condición y política, indicando su menor precisión. Los contrastes que sostengan conclusiones centrales deberán alcanzar al menos 100 repeticiones y una precisión Monte Carlo documentada.

**Timing de eventos:** comparar tres preparaciones en meses [1, 7, 13], [13, 19, 25] y [25, 31, 37], con celebración al mes siguiente y recursos comunes. Mantener el mismo número previsto de eventos ayuda a distinguir timing de cantidad; se reportarán los que realmente pudieron realizarse.

Se congelarán versión de código, configuración, comparaciones y semillas antes de las ejecuciones principales. Cualquier cambio posterior generará una nueva versión y no se mezclarán resultados incompatibles.

## 14. Análisis e interpretación

Se presentarán distribuciones, medias, medianas e intervalos de incertidumbre. Los contrastes entre políticas se calcularán por diferencias dentro de la misma repetición. Para intervalos mediante bootstrap se remuestrearán bloques completos de repeticiones con sus siete políticas; meses, agentes y enlaces de una misma ejecución no son réplicas independientes.

Las comparaciones centrales serán cada política frente a A y G frente a B–F. Se reportarán magnitudes y diferencias entre objetivos; si se realizan pruebas de hipótesis, se declarará el ajuste por comparaciones múltiples. El ranking medio no será el único resultado.

El Strategy Decision Map resumirá en qué condiciones una política ofrece mejores resultados comerciales, financieros o de alcance, junto con sus costos y límites. Una asociación entre un estado alcanzado y la acción elegida por G no prueba que esa acción fuera la mejor en ese estado. Para sostener recomendaciones por estado se necesitarán experimentos adicionales que clonen un mismo estado y comparen acciones factibles bajo futuros aleatorios emparejados. Esa extensión no es requisito para implementar v0.1; mientras no exista, el mapa se presentará como comparación por escenarios y reglas exploratorias.

No se interpretará la coincidencia con una expectativa teórica como validación empírica. La supervisión verificará coherencia conceptual y plausibilidad de los mecanismos. Los resultados se redactarán como evidencia de simulación bajo supuestos especificados.

## 15. Organización, responsables y primer entregable

| Ubicación | Entregable |
|---|---|
| `02_Especificacion_del_modelo` | Este documento, protocolo ODD desarrollado, diccionario y registro de cambios. |
| `03_Codigo/01_Modelo_NetLogo` | Modelo versionado. |
| `03_Codigo/02_Pruebas` | Pruebas automáticas y redes manuales. |
| `04_Diseno_de_experimentos` | Configuraciones, semillas, protocolos y reportes de verificación. |
| `05_Resultados_brutos` | Salidas originales por piloto, experimento principal y sensibilidad. |
| `06_Datos_procesados_y_analisis` | Datos derivados y resultados analíticos. |
| `07_Figuras_y_tablas` | Material editable y de publicación. |
| `08_Manuscrito` | Borradores y versión para envío. |
| `09_Replicacion_y_suplementos` | Instrucciones y paquete reproducible. |

El estudiante liderará la implementación, las pruebas, la organización de resultados, el análisis y el primer borrador. Los profesores orientarán la teoría, revisarán supuestos y verificación, y acompañarán la interpretación y revisión del manuscrito. La asistencia técnica debe quedar documentada y el estudiante deberá poder explicar las reglas y reproducir una ejecución.

**Siguiente tarea concreta:** implementar el módulo inicial de red, personas, necesidad, WOM orgánico, compra y capacidad; exportar una trayectoria y superar sus pruebas. Después se incorporarán referidos, incentivos, eventos y políticas, verificando cada módulo antes de integrarlo.

Esta especificación inicia la documentación ODD: propósito y entidades; interacción y decisiones; inicialización, reglas y agenda. El protocolo completo se actualizará con el código y sus decisiones definitivas. La tabla bibliográfica que vincule mecanismos con fuentes verificadas se desarrollará en paralelo; esta versión se fundamenta en la guía suministrada y no introduce referencias bibliográficas no verificadas.

### Registro de decisiones v0.1

Se proponen como elecciones revisables: compra única; presupuesto cerrado; servicio separado del tiempo de marketing; eventos internos con preparación anticipada; atribución uniforme entre exposiciones del mes; incentivos pagados solo por ventas atendidas del mismo mes; y adaptación basada en registros, no en conocimiento completo de la red. Cambiar cualquiera requiere registrar versión, motivo, consecuencias y experimentos que deben repetirse.
