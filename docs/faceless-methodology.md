# Skill: Faceless Content Methodology
# Metodología pura para contenido faceless (sin dependencias de proyectos)
# Versión: 2.1.0 (2026-08-09)

## Descripción
Metodología completa para crear contenido faceless monetizable. Aplica a CUALQUIER proyecto que quiera generar contenido sin mostrar rostro. Este es un skill GLOBAL, no está atado a ningún proyecto específico.

## Trigger
Cuando el usuario pida: crear contenido faceless, estrategia de video sin rostro, clipping pagado, monetización con reels/tiktoks.

## Framework: AI Loops
Todo entregable usa esta estructura:
- **Goal**: qué intentamos lograr
- **Verifier**: criterios de score explícitos (1-10)
- **Memory**: qué funcionó/falló
- **Stop rule**: N rondas o score umbral

## FASE 1: SELECCIÓN DE MOMENTOS (Clipping)
De cada pieza larga, identificar clips con:
- Hook natural en primeros 2 segundos
- Arco completo en 20-60 segundos (setup → tensión → payoff)
- Sentido total para quien no conoce al creador

## FASE 2: GENERACIÓN DE VIDEO
Stack recomendado:
- **Remotion**: video programático (React + TypeScript)
- **Componentes**: TitleSlide, ContentSlide, CodeSlide, VideoSlide, Caption
- **Render**: `pnpm exec remotion render [composition-id]`

## FASE 3: VOZ Y NARRACIÓN
- **Voicebox** (38K stars, MIT): clonación local sin costo
- **KittenTTS**: alternativa más ligera
- **ElevenLabs**: para alta calidad (costo)

## FASE 4: STORYTELLING (Dopamine Loop)
Framework 4 pasos:
1. **STAKES**: personaje + riesgo + urgencia
2. **BIG QUESTION**: contexto para pregunta jugosa
3. **HEADFAKE**: contrastar expectativas
4. **REHOOK**: reiniciar loop rápido

## FASE 5: DISTRIBUCIÓN
- Cadencia: 1-3 clips/día por cuenta
- Multi-plataforma: TikTok, Reels, Shorts
- Adaptar, no duplicar exacto

## FASE 6: MONETIZACIÓN (orden de prioridad)
1. Clipping pagado por marcas (pago por view/clip)
2. Acuerdos directos con creadores (retainer mensual)
3. Crecimiento de cuentas propias → afiliados
4. **PROHIBIDO**: depender de AdSense como ingreso principal

## FASE 7: CAPTURA (Audiencia Directa)
Todo funnel termina en activo propio:
- Email como fundación
- Substack (relación portable)
- Comunidades tipo Circle
- **PROHIBIDO**: depender de seguidores de plataforma

## Stack Herramientas
| Capa | Herramienta | Costo |
|------|-------------|-------|
| Video | Remotion | Gratis |
| Voz | Voicebox | Gratis |
| Distribución | Repurpose.io | $30/mes |
| Captura | Email + Substack | Gratis |

## 3 Técnicas de Consistencia de Video (v2.1.0)

La consistencia es el factor que separa contenido faceless "generado" de contenido
"producido". Tres tecnicas, tres capas distintas, combinables:

### Técnica 1 — Character Consistency (Seedance 2.5 R2V)
**Problema**: el personaje cambia de cara entre clips del mismo guion.
**Solución**: referencia-a-video (R2V). Seedance 2.5 (Dreamina) acepta hasta 50 inputs
multimodales (personaje + fondo + objetos) y mantiene la identidad entre clips.
- Hasta 4K, 30s por clip
- Generar un "character sheet" (3-5 tomas del mismo personaje) como referencia base
- Reusar la MISMA referencia en todos los clips de la serie

### Técnica 2 — Scene/World Consistency (Google Flow + world plates)
**Problema**: las escenas largas pierden coherencia (fondos, iluminación, continuidad).
**Solución**: world plates + keyframes.
- World plates: escena generada una vez, reusada como base para todas las tomas
- Keyframes de Flow (Veo-powered, credits diarios gratuitos): anclar inicio/fin de cada
  transición para que el modelo rellene de forma coherente
- Ideal para escenas largas y transiciones entre beats del guion

### Técnica 3 — Voice Consistency (voice blocks)
**Problema**: la voz del narrador/personaje cambia entre clips.
**Solución**: voice block fijo — un bloque de voz clonada (Voicebox local, gratis) o
voz de ElevenLabs contratada, generada UNA vez y reutilizada en todos los clips de la
serie. Nunca regenerar la voz por clip.

### Orquestación (pipeline Modo A)
```
Seedance 2.5 (personaje) → Flow (escenas largas) → Voice block (narración)
→ Lyria (banda sonora) → edición vía CutScript (transcripción)
```

### Regla de oro
Una técnica por clip NO basta: la consistencia es una propiedad del SERIE, no del clip.
Definir character sheet + world plate + voice block ANTES de generar el primer clip.

## Fuentes
- Research IG Batch 1-3 (32 screenshots)
- 15+ transcripts YouTube procesados
- Transcripts video IA 2026-08-03/05: `fMU7Ku8ma4A`, `3h89eX-p5sE`, `JWeSMMMJbts`
  (detalle completo en `PROMPT.md` §Video IA)

## Nota
- **Pivote estratégico**: AdSense-AI muere. Clipping pagado es el futuro.
- Este skill es METODOLOGÍA. La implementación depende de cada proyecto.
