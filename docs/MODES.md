<!-- ================================================================= -->
<!-- Development By Angel Esquivel (Automation) [AI-MONETIZE 2026] -->
<!-- ================================================================= -->
<!-- AI-MONETIZE - Generador de prompts de monetizacion IA -->

# Modos (A/B/C/D) - Definicion actual + mapeo legacy

Este repo ha reutilizado las letras A/B/C/D a lo largo del tiempo.

Para evitar confusion:
- La definicion oficial actual es la de `ai_monetize/modes.py` y `PROMPT.md`.
- Documentos viejos pueden mencionar nombres legacy (Character/Cine/Auto-Pilot).

## Definicion actual (2026-09)

- Modo A: Clipping / Contenido Faceless (pipeline original de media). [Ver veracidad](VERACIDAD_MMO.md#5-clipping--faceless-short-form)
- Modo B: AI Prescription (diagnostico/prescripcion de negocio). [Ver veracidad](VERACIDAD_MMO.md#8-ia--servicios-profesionales-ai-orchestrator)
- Modo C: Directorios Web (SEO pasivo). [Ver veracidad](VERACIDAD_MMO.md#6-local-lead-generation-modo-c)
- Modo D: Websites PyMEs (bundle determinista + retainer). [Ver veracidad](VERACIDAD_MMO.md#10-mapeo-a-modos-a-d-proyecto-ai-monetize)

## Ofertas candidatas (evaluacion 2026-10-21, NO implementadas)

Evaluadas contra los modos actuales a partir de la transcripcion
`ramiro-10-habilidades-ia-89oIFpQ4yyY.txt`. No hay codigo ni CLI para estas ofertas.

- AEO (SEO dentro de IAs): posicionar marcas en respuestas de ChatGPT/Perplexity.
  Prospeccion natural sobre empresas que ya pagan SEO tradicional. No la cubre Modo C
  (directorios), que es SEO pasivo propio. Estado: evaluacion.
- Meta Ads creativos: generar variantes inspiradas en la brecha de Facebook Ad Library
  (competidor con cientos de ads vs marca con pocos). Reusa el pipeline de video IA.
  Estado: evaluacion.
- Agente WhatsApp via LLM: agente conectado a un modelo que baja transcripciones de
  IG/YouTube, arma base de conocimiento del cliente y produce guiones en su voz.
  Estado: evaluacion.

Evaluacion coste-beneficio completa (puntajes del video, cruce contra los modos y
ranking propio): `docs/EVALUACION_OFERTAS.md`. Conclusion: Tier 1 = AEO, Meta Ads
creativos y productos digitales propios, en ese orden por velocidad de caja. Nada
se implementa sin decision explicita (Regla 5).

## Mapeo legacy (referencia)

Estos nombres aparecen en guias antiguas:

- Legacy "Modo B: AI Character" -> hoy Modo B se usa como AI Prescription.
- Legacy "Modo C: Cine Fusion" -> hoy Modo C se usa como Directorios Web.
- Legacy "Modo D: Auto-Pilot" -> hoy Modo D se usa como Websites PyMEs.

Si necesitas el comportamiento legacy (prompts/templates de character/cine/autopilot),
estan en `templates/` como material de referencia, no como definicion de negocio.

<!-- End Development By Angel Esquivel (Automation) [AI-MONETIZE 2026] -->
