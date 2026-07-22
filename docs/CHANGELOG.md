# CHANGELOG

## 1.0.2 (2026-07-16) — Publish Real + CLIP Token Fix

### Added
- `publish/` module — real social media publishing with simulated fallback
- `publish/providers/` — TikTok, YouTube, Instagram providers with API + simulated
- `publish/manager.py` — PublishManager with credential store, multi-platform routing
- `publish/providers/base.py` — SocialProvider ABC (httpx-based, rate limiting, auth)
- `publish/providers/types.py` — PublishContent, PublishResult, PostMetrics, AccountMetrics
- `--publish-real` CLI flag — forzar publicacion real (sin fallback simulated)
- `--publish-configure` CLI flag — configurar credenciales por plataforma
- `--publish-list` CLI flag — listar plataformas configuradas
- `docker-compose.yml` — agregado PostgreSQL 16 + Redis 7
- `pyproject.toml` — agregado httpx, psycopg2, sqlalchemy, redis como dependencias core
- `tests/test_publish.py` — 32 tests de providers, manager, stage integration

### Changed
- `stages/publish.py` — PublishStage usa PublishManager real en vez de `_simulate_publish()`
- `utils/prompt_diagnostic.py` — `_estimate_clip_tokens()` usa tokenizer CLIP real (openai/clip-vit-large-patch14) con lazy-load + fallback 1.45x

### Fixed
- Negative prompt en `modal_batch_diversidad.py` — acortado de 50w/96t a 23w/46t CLIP tokens
- Validacion de credenciales en providers: publish() no lanza excepcion si sin creds, va a simulated

## 1.0.1 (2026-07-09) — Krea2 Profiles + Cloud Render + Prompt Patterns

### Added
- `--krea2-profile` flag — perfiles predefinidos (recommended, turbo_*, raw_*)
- `--krea2-enhancer-strength` (0-2) y `--krea2-text-scale` (1.5-2.0) flags CLI
- `_KREA2_PROFILES` dict — 9 perfiles UNet/CLIP/VAE embebidos en CLI
- Wizard Krea2 interactivo en `--execute` con selector de perfil + modo manual
- `--non-interactive` flag — desactiva wizard
- `scripts/setup_comfyui_krea2.sh --enhancer` — instala ComfyUI-Krea2T-Enhancer
- `render/qa.py` — QA_RULES con 6 señales visuales de deteccion AI video
- `render/thumbnails.py` — ThumbnailSpec y specs por plataforma
- `render/backends.py` — CloudBackendRegistry (7 backends) con auto-deteccion + fallback
- `render/client.py` — DirectClient sync (requests + ws) + sidecar metadata .json
- `WorkflowBuilder.load_from_file()` — carga workflow JSON externo
- `--workflow`, `--cloud-status`, `--deploy-modal`, `--modal-gpu` flags CLI
- `scripts/deploy_comfyui_modal.py` — despliegue ComfyUI en Modal GPU T4/A10G/A100
- `scripts/deploy_comfyui_runpod.py` — crea pod GPU en RunPod con ComfyUI + Krea2
- `scripts/setup_colab.sh` — despliegue ComfyUI en Colab T4 gratis + cloudflared
- `/opt/harness-project/docs/transcripts/` — 13 referencias (11 videos + prompt patterns + index)
- `13_prompt_patterns_reference.md` — 25 pruebas documentadas, patrones SFW/NSFW, QA
- Regla global AGENTS.md: guardar transcripciones IA/tech automaticamente
- Modal GPU: 12 SFW + 12 NSFW imagenes generadas y validadas (0% censura)
- Video: 3 GIFs generados via SDXL grid frames → ffmpeg (Modal T4)

### Changed
- `_select_krea2_profile()` — wizard con 9 perfiles + modo manual
- Workflow overrides thread completo: CLI → engine → WorkflowBuilder
- `bridge.py` — documentados colab, kaggle, modal, runpod, seedance2, llama.cpp
- `engine.py` — usa CloudBackendRegistry + deploy_modal_comfyui() + auto-fallback
- Docs actualizadas (README badges/visual, CONFIG, TECHNICAL, GUIDE_WALKTHROUGH)
- `.dockerignore`: typo `!.README.md` → `!README.md`
- `.env` + `.env.template`: MODAL_TOKEN_ID, MODAL_TOKEN_SECRET, RUNPOD_API_KEY, COLAB_TUNNEL_URL, KAGGLE_TUNNEL_URL

## 1.0.0 (2026-07-08) — Licenciamiento + Render 2026 + Publicacion

### Added
- `LICENSE` — Apache 2.0 (D1 Open Core via licensed-one)
- `AUTHORS` — archivo de autoría
- `Wan 2.2` workflow template (Modo A/B — 24GB VRAM, Apache 2.0)
- `LTX-2.3` workflow template (Modo C — 12GB VRAM, video rápido con audio)
- `HunyuanVideo` workflow template (Modo C — calidad cinematográfica)
- `ReplicateBackend` — backend alternativo a ComfyUI (vía Replicate API)
- `RenderEngine.backend` — selector `comfyui|replicate|auto` en modo y .env
- `stages/publish.py` — PublishStage con soporte TikTok, YouTube, Instagram, Reddit
- `.env.template` — dummies para todas las plataformas + API keys
- `--publish` / `--platform` flags en CLI
- Warm-up scheduler en `RotationEngine.warmup_plan()` (7-10 días)
- `CONFIG.md` — guía completa de configuración

### Changed
- Version bump: 0.8.0 → 1.0.0
- Licencia: MIT → Apache 2.0 (D1 Open Core)
- `COST_PER_MODE` actualizado con costos 2026 (Wan/LTX más baratos)
- `Mode.stack` descriptions actualizadas con modelos 2026
- `render/workflows.py`: nuevos templates v2 conviven con v1
- `cli.py`: `--publish` y `--platform` integrados en pipeline
- `accounts/rotation.py`: warmup_plan() añadido al calendario

### Security
- Security audit: 0 shell=True, 0 hardcoded keys, .env en .gitignore

## 0.7.0 (2026-07-06) — Produccion + Docker + CI/CD

### Added
- `Dockerfile` — multi-stage build con python:3.12-slim (80MB final image)
- `docker-compose.yml` — API service con healthcheck, volumen persistente
- `.dockerignore` — exclude tests, venv, docs de la imagen
- `.gitignore` — exclude cache, env, projects, accounts
- `.github/workflows/test.yml` — GitHub Actions: pytest + integration + import validation

### Changed
- Version bump: 0.6.0 → 0.7.0

## 0.6.0 (2026-07-06) — Web API + FastAPI Server

### Added
- `ai_monetize/web/` — servidor REST API con FastAPI
  - `api.py` — 8 endpoints: /, /validate, /pipeline, /analytics, /accounts CRUD, /portfolio
  - `server.py` — Uvicorn runner con CLI integrado
- `tests/test_web.py` — 15 tests con TestClient (133 tests total)
- `--serve` / `--port` / `--host` flags CLI para iniciar servidor web
- `[web]` optional dependency group: `fastapi>=0.110`, `uvicorn>=0.29`, `pydantic>=2.5`

### Changed
- Version bump: 0.5.0 → 0.6.0

## 0.5.0 (2026-07-06) — Multi-cuenta + Portfolio

### Added
- `ai_monetize/accounts/` — modulo de gestion multi-cuenta
  - `account.py` — Account dataclass con perfil multi-plataforma
  - `manager.py` — AccountManager CRUD con persistencia JSON (~/.ai-monetize/accounts.json)
  - `rotation.py` — RotationEngine con calendario semanal y slots por plataforma
  - `portfolio.py` — PortfolioReport con revenue proyectado agregado
- `tests/test_accounts.py` — 26 tests (118 tests total)
- `--accounts` flag CLI — portfolio completo de cuentas
- `--multi N` flag — crear N cuentas en batch para multi-nicho
- `--account-add` / `--account-rm` CLI — CRUD de cuentas individuales
- Modo D (Auto-Pilot) integrado con multi-cuenta: `--multi` genera N cuentas con nivel auto

### Changed
- Version bump: 0.4.0 → 0.5.0

## 0.4.0 (2026-07-06) — Analytics + Dashboard

### Added
- `ai_monetize/analytics/` — modulo de analisis completo
  - `revenue_model.py` — RevenueForecast con estimaciones por nivel/nicho/modo
  - `trend_engine.py` — TrendProjection con proyecciones 30/60/90 dias
  - `dashboard.py` — DashboardReport con formato consola y export JSON
- `tests/test_analytics.py` — 28 tests (92 tests total)
- `--analytics` flag en CLI — dashboard independiente del pipeline
- `AnalyticsStage` — 7mo stage en pipeline (ANALYTICS)
- `docs/CHANGELOG.md` — historial del proyecto

### Changed
- Version bump: 0.3.0 → 0.4.0
- Pipeline ahora tiene 7 stages (VALIDATE → PLAN → CREATE → RENDER → DISTRIBUTE → MONETIZE → ANALYTICS)

## 0.3.0 (2026-07-06) — Motor de Render + Blueprint

### Added
- `ai_monetize/render/` — motor de render async completo
  - `ComfyUIClient` con queue, polling, batch, timeout recovery
  - `WorkflowBuilder` con 4 templates (A: FLUX/SDXL, B: IPAdapter+LoRA, C: AnimateDiff, D: batch)
  - `RenderEngine` orquestador con health check y manifest JSON
- `scripts/render.sh` — CLI directa para render
- `tests/test_render.py` — 19 tests (64 tests total)
- `docs/TECHNICAL.md` — blueprint completo para LLM

### Changed
- `engine.py`: scaffolding temprano del project_dir para RenderStage
- `stages.py`: todos los Stage.execute() aceptan project_dir
- RenderStage ejecuta asyncio.run(engine.render_project()) en modo real
- Version bump: 0.2.0 → 0.3.0

### Fixed
- `test_integration.sh`: `((VAR++))` → `VAR=$((VAR + 1))` (bash set -e gotcha)
- ComfyUI client session cleanup (no mas "Unclosed client session" warnings)

## 0.2.0 (2026-07-05) — Niveles + Templates + Tests

### Added
- `data/template_prompts.py` — render_prompt() generador
- `models/niche.py` — mode-aware score_to_level + MODE_LEVEL_ADJUSTMENT
- `validator.py` — mode_id parameter, nivel mode-aware
- `tests/` — 45 tests pytest (validator, pipeline, templates)

### Changed
- Todos los stages reciben level consistente
- CLI --level flag
- Pipeline engine resuelve nivel automatico o por override
- RenderStage escala imagenes/videos por nivel

## 0.1.0 (2026-07-04) — Autocontencion + Datos

### Added
- Proyecto autocontenido en /opt/ai-monetize/
- 120 nichos en Green Grid con 6-factor scoring
- 13 niche templates × 4 niveles
- 4 modos de uso (A/B/C/D)
- Pipeline de 6 stages
- 15 integration tests bash
- bridge.py reconvertido a autodiagnostico
