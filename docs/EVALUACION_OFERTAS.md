# =================================================================
# AI-MONETIZE - Evaluacion coste-beneficio de ofertas candidatas

# Evaluacion de Ofertas Candidatas

Evaluacion coste-beneficio de las 10 habilidades del mercado (fuente:
`ramiro-10-habilidades-ia-89oIFpQ4yyY.txt`) cruzadas contra lo que el proyecto ya
construye. Objetivo: decidir que se agrega al catalogo por relacion
ingreso-teorico / esfuerzo / riesgo. Este documento NO autoriza implementacion:
ningun item se convierte en codigo sin decision explicita (Regla 5).

## 1. Metodo

1.1. Variables de puntaje del autor del video (escala 1-5 cada una, maximo 25):

1.1.1. Facilidad: que tan facil es arrancar sin experiencia.
1.1.2. Velocidad: que tan rapido llega el primer cliente.
1.1.3. Validacion: cuanta gente ya esta cobrando por esto.
1.1.4. Competencia: mas estrellas = menos saturado.
1.1.5. Largo plazo: si reemplaza un sueldo o es moda pasajera.

1.2. Calidad de evidencia (etiqueta obligatoria en cada cifra):

1.2.1. `[V]` verificado en el proyecto (codigo, guia o test existente).
1.2.2. `[V+]` verificado externamente (fuente primaria consultada).
1.2.3. `[~]` auto-reportado por el autor del video, no verificable.
1.2.4. `[?]` inferencia propia sin evidencia, marcada como hipotesis.

1.3. Criterios propios de coste-beneficio (peso sobre la decision final):

1.3.1. Ingreso teorico por cliente y recurrencia (mensual vs unico).
1.3.2. Costo de adquisicion: requiere prospeccion en frio, red o contenido.
1.3.3. Reuso de activos ya construidos en el proyecto (video IA, leads,
       websites, publicacion multicanal, analytics, escalado).
1.3.4. Riesgo operativo: dependencia de plataformas, bloqueo de cuentas,
       necesidad de cuenta publicitaria propia.
1.3.5. Escalabilidad sin sumar horas propias (sistemico vs artesanal).

1.4. Regla de honestidad: las cifras de ingresos del video son
     auto-reportadas `[~]` y no se usan como base de proyeccion. Se usan solo
     para ordenar y para estimar techos teoricos, nunca como garantia.

## 2. Puntaje del mercado (datos encontrados en el video)

2.1. Habilidad 1 — Anuncios con IA para Meta (Facebook/Instagram) `[~]`.
     2.1.1. Puntaje 20/25 (fac. 3, vel. 3, val. 5, comp. 4, largo 5).
     2.1.2. Precio declarado: 1,000-5,000 USD por marca, cobro mensual `[~]`.
     2.1.3. Prospeccion: brecha en Facebook Ad Library (competidor con ~700
            anuncios vs marca con ~20) `[~]`.
     2.1.4. Prueba social del autor: paga por creativos IA propios `[~]`.

2.2. Habilidad 2 — Escritura de contenido con IA + agente WhatsApp `[~]`.
     2.2.1. Puntaje 22/25 (fac. 4, vel. 5, val. 5, comp. 3, largo 5).
     2.2.2. Mecanismo: agente en WhatsApp conectado a un modelo que ingesta
            transcripciones de IG/YouTube y arma documento maestro + guiones
            en la voz del cliente `[~]`.
     2.2.3. Modelo comercial: contenido gratis a cambio de porcentaje de
            ventas `[~]`.

2.3. Habilidad 3 — Creador de contenido sobre IA (marca propia) `[~]`.
     2.3.1. Puntaje 18/25 (fac. 2, vel. 3, val. 5, comp. 3, largo 5).
     2.3.2. Cuenta declarada: 83,000 seguidores en ~2 meses, un video con 3
            millones de vistas `[~]`.
     2.3.3. Inconsistencia detectada en el relato: se mencionan "casi 100,000
            comentarios" y luego "50,000 personas escribiendome" para el mismo
            video `[~]`. Marca de veracidad debil; no usar para proyeccion.

2.4. Habilidad 4 — Shadow operator (socio de operaciones) `[~]`.
     2.4.1. Puntaje 19/25 (fac. 2, vel. 3, val. 5, comp. 4, largo 5).
     2.4.2. Comision declarada: 15%-50% de las ventas `[~]`.
     2.4.3. Prospeccion: cuentas de 50,000-100,000 seguidores `[~]`.

2.5. Habilidad 5 — Agencia de YouTube `[~]`.
     2.5.1. Puntaje 19/25 (fac. 2, vel. 3, val. 5, comp. 4, largo 5).
     2.5.2. Comision declarada: 1,000-5,000+ USD por mes `[~]`.
     2.5.3. Argumento de plataforma: el video es el unico formato que sigue
            mostrandose durante años `[?]` (razonable, no medido aqui).

2.6. Habilidad 6 — Multicuentas + clipping `[~]`.
     2.6.1. Sin puntaje en el corte del video `[~]`.
     2.6.2. Argumento de riesgo: bloqueos de cuentas de Meta como motivo para
            diversificar `[~]`.

2.7. Habilidad 7 — SEO dentro de las IAs (AEO) `[~]`.
     2.7.1. Puntaje 23/25 (fac. 4, vel. 4, val. 5, comp. 5, largo 5); el mas
            alto de la lista. El autor lo describe como "oceano azul" `[~]`.
     2.7.2. Prospeccion: empresas que ya pagan SEO tradicional (presupuesto
            existente y educado) `[~]`.
     2.7.3. Precio declarado: "miles de dolares al mes" sin cifra concreta
            `[~]`.

2.8. Habilidad 8 — Generacion de prospectos con IA `[~]`.
     2.8.1. Puntaje 20/25 (fac. 5, vel. 3, val. 5, comp. 2, largo 5).
     2.8.2. Flujo demostrado: pedir por audio 100 negocios de Google Maps de
            una ciudad, con telefono, web, rating y Place ID, en Sheets `[~]`.
     2.8.3. Competencia 2/5 declarada: el peor de la lista porque el comprador
            tipico ya compra listas a proveedores establecidos `[~]`.

2.9. Habilidad 9 — Asistente de chat para negocios `[~]`.
     2.9.1. Puntaje 19/25 (fac. 3, vel. 5, val. 5, comp. 3, largo 3).
     2.9.2. Uso: calificar, responder y agendar en automatico en IG/web `[~]`.
     2.9.3. Largo plazo 3/5 declarado por el propio autor `[~]`.

2.10. Habilidad 10 — Productos digitales propios con IA `[~]`.
      2.10.1. Sin puntaje numerico en el corte; el autor la declara ganadora
             ("compite en su propia categoria") `[~]`.
      2.10.2. Captura declarada: una tienda con 20,000 USD en 80 dias, unos
             250 USD por dia `[~]`.
      2.10.3. Modelo: crear una vez, vender infinito, sin stock ni logistica
             `[~]`.

## 3. Cruce contra lo ya construido

3.1. Cobertura actual de cada habilidad por el proyecto:

| # | Habilidad | Encaje en el proyecto | Estado |
|---|-----------|----------------------|--------|
| 1 | Meta Ads creativos | Modo de video IA + plantillas + publicacion | Gap |
| 2 | Ghostwriting + agente WhatsApp | Modo D (leads, audit) parcial | Gap |
| 3 | Creador de contenido IA | Pipeline faceless P1 | Parcial |
| 4 | Shadow operator | Modo B (operacion de negocio) es lo mas cercano | Gap |
| 5 | Agencia de YouTube | P8 define criterio faceless | Parcial |
| 6 | Multicuentas + clipping | Modo A + guias anti-deteccion | Cubierto |
| 7 | AEO / SEO en IAs | Modo C (directorios) no lo cubre | Gap |
| 8 | Prospeccion con IA | Modo D `--leads-file` + `--audit` | Parcial |
| 9 | Asistente de chat | No cubierto (AI Receptionists es telefonia) | Parcial |
| 10 | Productos digitales | El proyecto construye activos de otros | Gap |

3.2. Activos reutilizables si se implementa cualquiera de las 3 prioritarias
     `[V]`:

3.2.1. `ai_monetize/render/` — motor de video (ComfyUI/Replicate) y workflows
       por modo: insumo directo para Meta Ads creativos.
3.2.2. `ai_monetize/leads/` + `docs/LEAD_SOURCING.md` — prospeccion: insumo
       para Meta Ads y AEO (misma lista de empresas que ya pagan marketing).
3.2.3. `ai_monetize/websites/` — generacion y auditoria de sitios: insumo para
       AEO (los activos publicos que las IAs indexan) y para productos digitales
       (la pagina que cobra y entrega).
3.2.4. `ai_monetize/publish/` — publicacion multicanal (Postiz/OAuth): insumo
       para distribucion de cualquier oferta.
3.2.5. `ai_monetize/analytics/` + `scaling/` — scoring y pronostico: base para
       medir la propia oferta.

## 4. Ranking coste-beneficio (criterio propio)

4.1. Formula cualitativa aplicada (1.3.1-1.3.5), sin inventar montos. El
     orden pondera: caja rapida + recurrencia + reuso de activos + riesgo.

4.2. Tier 1 — mejor relacion esfuerzo/caja con activos ya existentes:

4.2.1. **AEO (habilidad 7)**. Razon: puntaje mas alto del video (23/25) `[~]`,
       competencia declarada 5/5 (oceano azul) `[~]`, publico que ya paga
       posicionamiento `[~]`, y el proyecto ya genera y audita sitios web
       `[V]` (los artefactos que las IAs leen). Coste de entrada bajo: es
       servicio con entregable documental, no requiere GPU ni cuenta
       publicitaria.
4.2.2. **Meta Ads creativos (habilidad 1)**. Razon: recurrencia mensual
       declarada `[~]`, validacion 5/5 `[~]`, y el pipeline de video IA ya
       existe `[V]`; la prospeccion tiene un disparador objetivo y verificable
       (brecha de anuncios en Ad Library, publica y gratis) `[~]`. Coste: exige
       entender anuncios (facilidad 3/5) y, para el propio cliente, cuenta
       publicitaria.
4.2.3. **Productos digitales propios (habilidad 10)**. Razon: unico activo
       propio (ingreso no atado a horas), coincide con el pivote estrategico
       del proyecto `[V]`, y el proyecto ya tiene web que cobra y entrega
       `[V]`. Coste: requiere capital de anuncios para validar y curva de
       tiempo mayor antes de la primera venta.

4.3. Tier 2 — buenas pero con dependencia de terceros o de tiempo propio:

4.3.1. Ghostwriting + agente WhatsApp (2): caja rapida `[~]`, pero exige
       mantener un agente por cliente y una dependencia de proveedor LLM
       (coste variable por uso, ver Regla G5 antes de elegir proveedor).
4.3.2. Agencia de YouTube (5) y Shadow operator (4): techo alto `[~]` pero
       facilidad 2/5 y comision dependiente del exito del cliente; se apoyan
       parcialmente en activos existentes (video, publish).
4.3.3. Asistente de chat (9): velocidad 5/5 `[~]`, largo plazo 3/5 declarado
       por el propio autor `[~]`; compite con herramientas ya empaquetadas.

4.4. Tier 3 — menor prioridad:

4.4.1. Prospeccion con IA (8): facilidad alta pero competencia 2/5 declarada
       `[~]`; el proyecto ya lo cubre parcialmente `[V]`, agregarlo aporta poco
       margen nuevo.
4.4.2. Creador de contenido sobre IA (3): exige marca personal y la evidencia
       declarada tiene una inconsistencia (`[~]`, 2.3.3); es canal de
       adquisicion, no oferta de servicio.
4.4.3. Multicuentas + clipping (6): ya cubierto por Modo A `[V]`.

4.5. Conclusion del ranking: la combinacion con mejor coste-beneficio
     teorico es **AEO como oferta de entrada + Meta Ads como recurrencia +
     productos digitales propios como activo de largo plazo**, secuenciada en
     ese orden por velocidad de caja. Hipotesis `[?]`: las tres comparten el
     mismo insumo (una empresa con presupuesto de marketing), lo que permite
     vender AEO, medir resultado y ofrecer despues los creativos.

## 5. Costos y riesgos a resolver antes de implementar

5.1. Veracidad de mercado `[~]`: ningun monto del video esta verificado. Antes
     de cotizar, contrastar con 3 fuentes primarias (precios publicos de
     agencias que ya venden AEO y creativos IA) y registrar en
     `docs/VERACIDAD_MMO.md`.
5.2. AEO: definir que se entrega y como se mide (que consultas, que motores,
     con que frecuencia). Sin linea base no hay renovacion.
5.3. Meta Ads: definir si el entregable son los assets o la gestion de la
     campana; lo segundo implica acceso a cuentas de terceros y
     responsabilidad sobre el gasto (requiere criterios de "done" y limites,
     guardrail G3).
5.4. Productos digitales: capital de validacion y decision de nicho; el
     proyecto puede fabricar el producto y la pagina, pero el riesgo comercial
     es propio del dueno.
5.5. Dependencia de plataformas: cualquier oferta sobre Meta o Google hereda
     riesgo de bloqueo o cambio de politica `[~]`.
5.6. Coste de mantenimiento: cada oferta nueva que se implemente en el proyecto
     suma superficie de codigo y un modo mas que mantener.

## 6. Estado y criterio de activacion

6.1. Estado actual: las 10 habilidades se mantienen como **evaluadas y no
     implementadas**. Este documento no habilita trabajo de codigo (Regla 5).

6.2. Criterio de activacion propuesto para pasar de evaluacion a
     implementacion:

6.2.1. Elegir **una sola** oferta (el video lo marca como el error clasico:
       tres a la vez y ninguna cobrada) `[~]`.
6.2.2. Validar demanda sin codigo: 5 conversaciones con cuentas que ya paguen
       la oferta equivalente, y registrar respuesta.
6.2.3. Definir metrica de "done" y precio antes de escribir la primera linea
       (Regla N2 / proceso verificable).
6.2.4. Si la oferta elegida toca cuentas o dinero de terceros, aplicar G3
       (limites, revisor G1, criterios verificables).

6.3. Prioridad declarada por el dueno (2026-10-21): documentar al 100% y
     posponer implementacion. Este documento cumple esa fase.

# =================================================================
