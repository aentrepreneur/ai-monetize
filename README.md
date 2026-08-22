# AI-Monetize

> Pipeline de monetización AI: de la idea al ingreso.

[![Python](https://img.shields.io/badge/Python-3.12+-3776AB?logo=python)](https://python.org)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue)](LICENSE)
[![CI](https://github.com/anomalyco/ai-monetize/actions/workflows/test.yml/badge.svg)](https://github.com/anomalyco/ai-monetize/actions/workflows/test.yml)
[![Tests](https://img.shields.io/badge/Tests-192%20passing-brightgreen)](tests/)

## Stack

`FastAPI · SQLAlchemy · Celery · Redis · PostgreSQL · httpx · pytest · Docker`

## Quick start

```bash
# Instalar
git clone https://github.com/anomalyco/ai-monetize && cd ai-monetize
python3 -m venv venv && source venv/bin/activate
pip install -e .[full]

# Configurar
cp .env.template .env  # llenar API keys

# Probar
bash scripts/run_tests.sh

# Servidor web
python3 -m ai_monetize --serve
# → http://localhost:8000  (API)
# → http://localhost:8000/docs  (Swagger)
# → http://localhost:8000/monitor  (Dashboard)
```

## Pipeline (8 etapas)

| # | Etapa | Qué hace |
|---|-------|----------|
| 1 | **VALIDATE** | Scorea el nicho contra Green Grid (6 factores) |
| 2 | **PLAN** | Elige template, genera character bible, scaffolding |
| 3 | **CREATE** | Genera scripts, prompts, storyboards por nivel |
| 4 | **RENDER** | Renderiza video/imagen (ComfyUI / Replicate / Modal) |
| 5 | **DISTRIBUTE** | Prepara assets por plataforma, thumbnails, QA visual |
| 6 | **PUBLISH** | Publica a 5 plataformas (TikTok, YouTube, Instagram, Reddit, X/Twitter) |
| 7 | **MONETIZE** | Revenue forecast, proyecciones 30/60/90 días |
| 8 | **ANALYTICS** | Dashboard, tendencias, reporte consolidado |

## 5 Providers de publicación

| Plataforma | Auth | Post types |
|------------|------|------------|
| TikTok | OAuth2 | video |
| YouTube | OAuth2 | video, short |
| Instagram | OAuth2 | image, video, carousel, reel, story |
| Reddit | OAuth2 | text, link, image, video |
| X/Twitter | OAuth 2.0 | text, image, video, link |

## Integraciones externas

- **Postiz** — scheduling y analytics multi-plataforma
- **OpenMontage** — 12 pipelines de producción de video (AGPL-3.0)

## Estructura

```
ai_monetize/
├── publish/providers/   # 5 SocialProvider concretos
├── web/                 # FastAPI (16 endpoints + /health + /monitor)
├── integrations/        # Postiz, OpenMontage
├── analytics/           # RevenueForecast, TrendProjection, DashboardReport
├── accounts/            # Multi-cuenta, rotación, portfolio
├── render/              # ComfyUI, Replicate, Modal, WorkflowBuilder v1+v2
├── core/                # CycleEngine, PipelineEngine, 8 stages
├── utils/               # Logger JSON, prompt_diagnostic
├── tasks/               # Celery workers (publish scheduling)
├── models/              # NicheTemplate, CharacterBible, Level
└── data/                # Green Grid, template_prompts
```

## Licencia

Apache 2.0 — D1 Open Core via [licensed-one](https://github.com/anomalyco/opencode/skills/licensed-one).
