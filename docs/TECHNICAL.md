# AI-MONETIZE — Blueprint Tecnico para LLM

## 1. QUE ES ESTE PROYECTO

```pseudocode
INPUT:  una idea de nicho (string) + modo de uso (A/B/C/D) + nivel opcional
OUTPUT: proyecto scaffold con plan de ejecucion, prompts por nivel,
        estimacion de costos, workflow ComfyUI, y revenue model

VALOR:  validar un nicho en 0.1s sin gastar tokens APIs,
        escalar a 100+ nichos sin crear archivos individuales,
        generar assets reales via ComfyUI cuando esta disponible
```

Es un sistema de **validacion y generacion de proyectos de contenido AI**. No es un wrapper de APIs ni un template engine estatico. Es un pipeline con estado que transforma una idea en un proyecto ejecutable.

---

## 2. ARQUITECTURA GENERAL

```
CLI (argparse) 
  │
  ▼
PipelineEngine.run(idea, dry_run, level)
  │
  ├──► resolve level (validate o override)
  ├──► scaffold project dir (execute mode)
  │
  ├──► ValidateStage   — Green Grid + word boundaries + modo-aware
  ├──► PlanStage       — budget, timeline, stack, revenue
  ├──► CreateStage     — prompts generados por nivel+modo
  ├──► RenderStage     — ComfyUI/Replicate client (v2 templates 2026)
  ├──► DistributeStage — estrategia: plataformas, horarios, growth tactics, warm-up
  ├──► PublishStage    — ejecucion: posting en TikTok, YT, IG, Reddit
  ├──► MonetizeStage   — pricing tiers, income streams, platform fees
  ├──► AnalyticsStage  — RevenueForecast + TrendProjection + Dashboard
  │
  └──► scaffold project files (PLAN.md + pipeline.json)
```

### 2.1 Capas del paquete `ai_monetize/`

```
ai_monetize/
├── models/          # Dataclasses simples (Level, NicheTemplate, CharacterBible, ProjectScaffold)
│   ├── niche.py     # Level enum + score_to_level() + MODE_LEVEL_ADJUSTMENT
│   ├── character.py # CharacterBible para modo B
│   └── project.py   # ProjectScaffold para scaffolded output
│
├── data/            # Datos embebidos (no lee archivos externos)
│   ├── green_grid.py   # 120 nichos con 6 factores de scoring
│   ├── niches.py       # 13 templates con 4 niveles cada uno
│   ├── template_prompts.py  # render_prompt(niche, level, mode) → string
│   └── tools.py       # 12 guias de herramientas
│
├── stages/          # 8 Stage classes modulares
│   ├── __init__.py  # STAGES list ordenada
│   ├── registry.py  # StageID, Stage, StageResult, COST_PER_MODE
│   ├── publish.py   # PublishStage (TikTok/YT/IG/Reddit)
│   └── ...          
│
├── render/          # Motor de render async
│   ├── client.py    # ComfyUIClient + ReplicateBackend
│   ├── workflows.py # WorkflowBuilder + MODE_WORKFLOWS v1/v2
│   └── engine.py    # RenderEngine con backend selector
│
├── analytics/       # Revenue model + trend projection + dashboard
│   ├── revenue_model.py # RevenueForecast por nivel/nicho/modo
│   ├── trend_engine.py  # TrendProjection 30/60/90d
│   └── dashboard.py     # DashboardReport + export JSON
│
├── accounts/        # Multi-cuenta management
│   ├── account.py   # Account dataclass
│   ├── manager.py   # CRUD + JSON persistence
│   ├── rotation.py  # RotationEngine + calendar + warmup_plan()
│   └── portfolio.py # PortfolioReport
│
├── web/             # FastAPI REST server
│   ├── api.py       # 8 endpoints
│   └── server.py    # Uvicorn runner
│
├── modes.py         # 4 Modos de Uso (A/B/C/D) como dataclasses frozen
├── validator.py     # Green Grid validator + heuristic fallback
├── engine.py        # PipelineEngine orquesta todo
├── cli.py           # argparse + menu interactivo + --publish/--platform
├── bridge.py        # Autodiagnostico
└── __init__.py      # Exporta todo, version 1.0.0
```

### 2.2 Reglas arquitectonicas estrictas

```python
# PROHIBIDO: imports entre capas incorrectas
# Correcto:  scripts/ → core/ → tools/ → security/ → utils/
# Capa superior puede importar de inferiores
# PROHIBIDO: utils importando de nadie
# PROHIBIDO: security importando de tools/core/scripts
# PROHIBIDO: tools importando de core/scripts
# PROHIBIDO: core importando de scripts
```

---

## 3. CICLO DE VIDA DE UNA IDEA

### 3.1 Entrada

```python
class Idea:
    niche: str      # "fitness yoga mujeres 40+"
    mode_id: str    # "A" | "B" | "C" | "D"
    concept: str    # opcional: description adicional
    budget: float   # opcional: presupuesto mensual
```

### 3.2 Validacion

```python
def validate(niche, mode_id="A") -> ValidationResult:
    1. Normalizar: niche.lower()
    2. Iterar GREEN_GRID ordenado por longitud descendente
    3. Para cada keyword, aplicar _word_match():
       - si tiene espacio: substring match exacto
       - si no: regex con \b word boundaries
    4. Si match: usar score del GridEntry
    5. Si no match: heuristico 55 + hash % 21
    6. score_to_level(score, mode_id) → ajusta por modo
    7. Devolver ValidationResult con factores, nivel, color
```

### 3.3 Pipeline

```python
for stage in [ValidateStage, PlanStage, CreateStage, RenderStage,
              DistributeStage, PublishStage, MonetizeStage, AnalyticsStage]:
    result = stage.execute(niche, mode, dry_run, level, project_dir)
    # Cada stage devuelve StageResult con:
    #   - stage: StageID enum
    #   - status: OK | SKIP | WARN | ERROR
    #   - message: string para output humano
    #   - details: dict con datos estructurados
    #   - cost_avoided: float (dry-run)
    #   - tokens_saved: int (dry-run)
    #   - time_saved_min: int (dry-run)
```

### 3.4 Dry-run vs Execute

```
dry_run=True:
  - No toca APIs externas
  - No genera assets
  - Calcula costos evitados ($X en APIs)
  - Muestra resumen con nivel, score, timeline

dry_run=False:
  - Crea project_dir con estructura de carpetas
  - Genera 01_PLAN.md con datos de todos los stages
  - Escribe pipeline.json con resultado estructurado
  - RenderStage intenta conectar con ComfyUI (si disponible)
```

---

## 4. MODELO DE DATOS

### 4.1 Level (niche.py)

```python
Level.BEGINNER      = 0-59   # score + adjustment
Level.INTERMEDIATE  = 60-74
Level.ADVANCED      = 75-84
Level.EXPERT        = 85-100

MODE_LEVEL_ADJUSTMENT = {
    "A": 0,    # Faceless: neutral
    "B": -5,   # Character: mas dificil
    "C": -5,   # Cine: mas dificil
    "D": 15,   # Auto-Pilot: mas permisivo
}

score_to_level(score, mode_id):
    adjusted = score + MODE_LEVEL_ADJUSTMENT[mode_id]
    clamped = max(0, min(100, adjusted))
    # buscar en LEVEL_RANGES
```

### 4.2 Green Grid (green_grid.py)

```python
GridEntry:
    niche: str          # keyword para matching
    trend: int 0-100    # peso 0.25 — tendencia actual
    monetization: 0-100 # peso 0.25 — potencial de ingreso
    competition: 0-100  # peso 0.20 — nivel de competencia
    repeatability: 0-100# peso 0.15 — se puede repetir contenido
    faceless: 0-100     # peso 0.10 — apto para faceless
    beginner: 0-100     # peso 0.05 — facil para empezar

    score = trend*0.25 + monetization*0.25 + competition*0.20
          + repeatability*0.15 + faceless*0.10 + beginner*0.05

    color = GREEN >= 75 | YELLOW >= 50 | RED < 50
```

Hay **120 entradas** en GREEN_GRID cubriendo: fitness, finanzas, gaming,
cocina, viajes, tecnologia, arte, animals, deportes, lifestyle, etc.

### 4.3 Niche Templates (niches.py)

13 templates con estructura de 4 niveles:

```python
NicheTemplate:
    name: str           # "Fitness/Gym"
    keywords: [str]     # ["fitness", "gym", "ejercicio", ...]
    description: str    # "Rutinas, consejos de entrenamiento..."
    levels: dict[Level, dict]:
        Level.BEGINNER:
            videos_week: "1-3"
            revenue: "$0 (testing)"
            tools: [MoneyPrinterTurbo, Pexels, ...]
            timeline: "0-2 meses"
            prompt_template: "Genera un script..."
```

### 4.4 Modos de Uso (modes.py)

```
MODO A — Faceless Channels
  Costo: $0-60/mes | Timeline: 4-6 meses | Riesgo: Bajo
  Stack: LTX-2.3 (12GB VRAM), Claude/Groq, Pexels, ElevenLabs, DaVinci
  Templates: 13 prompts base

MODO B — AI Character
  Costo: $0-30/mes | Timeline: 4-6 meses | Riesgo: Medio
  Stack: Wan 2.2 (24GB VRAM, Apache 2.0), ComfyUI, Kaggle GPU, LoRA, Passes/RM11
  Templates: 25 prompts base

MODO C — Cine Fusion
  Costo: $20-100/mes | Timeline: 3-5 meses | Riesgo: Medio-Alto
  Stack: HunyuanVideo (cine, A100+), Wan 2.2 14B, Real-ESRGAN, DaVinci, ElevenLabs
  Templates: 38 prompts base

MODO D — Auto-Pilot
  Costo: $40-180/mes | Timeline: 2-4 meses | Riesgo: Alto
  Stack: Wan 2.2 batch (Replicate API), N8N, Postiz/Buffer, Claude/Groq, R2
  Templates: 50 prompts base
```

---

## 5. MOTOR DE RENDER

### 5.1 ComfyUIClient

```python
class ComfyUIClient:
    # CONFIG
    base_url = "http://127.0.0.1:8188"
    timeout = 300s
    poll_interval = 2s
    max_retries = 3

    # METODOS
    async health_check() -> bool
        GET /system_stats, timeout 10s
        usa aiohttp.ClientSession() temporal (no _get_session)
        si no hay aiohttp: usa _get_session() (fallback urllib)

    async queue_prompt(workflow, retry=0) -> prompt_id
        POST /prompt con payload {"prompt": workflow}
        retry con backoff exponencial 2^attempt
        raise TransportError si falla todos

    async poll_result(prompt_id) -> RenderJob
        GET /history/{prompt_id} cada poll_interval segundos
        detecta completed: True en status
        detecta progress: messages con type "progress"
        timeout total: self.timeout segundos → TIMEOUT

    async render_batch(workflows, max_concurrent=2) -> [RenderJob]
        semaphore asyncio.Semaphore(max_concurrent)
        as_completed para resultados parciales
```

### 5.2 WorkflowBuilder

Dos familias de templates: v1 (legacy SDXL/AnimateDiff) y v2 (2026).

```python
MODE_WORKFLOWS = {   # v1 — legacy
    "A": Faceless Image — FLUX/SDXL, 1024x1024, 20 steps
    "B": Character Image — SD15 + IPAdapter FaceID + LoRA
    "C": Cinematic Video — AnimateDiff + ControlNet
    "D": Batch Image — reusa workflow de A
}

MODE_WORKFLOWS_V2 = {   # v2 — 2026
    "A": Faceless LTX-2.3 — video rapido, 12GB VRAM, bajo costo
         5 nodos: LTXVideoModelLoader, LTXVideoPromptEncode,
         LTXVideoSampler, VAEDecode, VideoCombine

    "B": Character Wan 2.2 — 24GB VRAM, Apache 2.0
         7 nodos: WanVideoLoader, WanVideoTextEncode, LoadImage,
         WanVideoSampler, VAEDecode, VideoCombine

    "C": Cinema HunyuanVideo — cinematografico, A100/H100
         6 nodos: HunyuanVideoLoader, HunyuanVideoTextEncode,
         HunyuanVideoSampler, VAEDecode, VideoCombine

    "D": Auto-Pilot Wan Batch — via Replicate API
         {deployment, version, input} con prompt/batch_size
}

LEVEL_OVERRIDES:
    BEGINNER:      steps=15, cfg=7.0, width=512,  height=512
    INTERMEDIATE:  steps=20, cfg=6.0, width=768,  height=768
    ADVANCED:      steps=25, cfg=5.0, width=1024, height=1024
    EXPERT:        steps=30, cfg=4.5, width=1024, height=1024

WorkflowBuilder.build(mode_id, prompt, level, version="v1", **overrides):
    1. elegir fuente segun version (v1 → MODE_WORKFLOWS, v2 → MODE_WORKFLOWS_V2)
    2. deepcopy del workflow template
    3. reemplazar {{prompt}}, {{negative_prompt}}, {{style}} en todos los nodos
    4. aplicar LEVEL_OVERRIDES al primer sampler (KSampler/WanVideoSampler/...)
    5. aplicar **overrides
    6. devolver dict JSON-serializable
```

### 5.3 RenderEngine

```python
RenderEngine(backend="auto", version="v2"):
    backend = "comfyui" | "replicate" | "auto" (segun template)
    version = "v1" (legacy) | "v2" (Wan/LTX/Hunyuan)

RenderEngine.render_project(...) -> RenderResult

    dry_run=True:
        - usa SCALE_BY_LEVEL para imagenes/videos
        - status = "dry_run"

    dry_run=False:
        - _resolve_backend(template): si auto, usa el field "backend" del template
        - si comfyui: health check, render_batch con max_concurrent
        - si replicate: ReplicateBackend.predict(deployment, version, input)
        - escribe render_manifest.json
        - status = "completed" | "partial"

    FINALLY: cerrar cliente si no fue provisto externamente
```

### 5.4 ReplicateBackend

```python
class ReplicateBackend:
    API_BASE = "https://api.replicate.com/v1"

    async predict(deployment, version, input_data) -> RenderJob
        1. POST /predictions con body {version, input}
        2. Poll GET /predictions/{id} cada poll_interval
        3. Status: starting → processing → succeeded | failed
        4. Timeout configurable (default 600s)
        5. Extrae progress de logs si disponible
```

---

## 6. SCALING POR NIVEL

```python
SCALE_BY_LEVEL = {
    BEGINNER:      {images:10,  videos:5,  cost_mult:1}
    INTERMEDIATE:  {images:25,  videos:10, cost_mult:2}
    ADVANCED:      {images:50,  videos:20, cost_mult:3}
    EXPERT:        {images:100, videos:40, cost_mult:5}
}

COST_PER_MODE = {  # Actualizado 2026 (Wan/LTX reducen costos)
    "A": {create:0.35, render_image:0.01, render_video:0.015}
    "B": {create:1.50, render_image:0.015, render_video:0.04}
    "C": {create:4.00, render_image:0.03, render_video:0.08}
    "D": {create:6.00, render_image:0.008, render_video:0.015}
}
```

---

## 7. KEY DECISIONS (NO CAMBIAR SIN ENTENDER)

### 7.1 Word Boundaries en Validator

```python
# CRITICO: \bai\b NO debe matchear dentro de "daily"
# Solucion: \b{re.escape(kw)}\b para keywords sin espacio
# Keywords con espacio: substring exacto (ej: "ai tools" in "ai tools automation")
```

### 7.2 Templates generados vs archivos markdown

```python
# Los prompts viven en data/niches.py como datos Python
# Los .md en templates/niche_templates/ son exportables (generados)
# NO son fuente primaria
# Razon: sostenible para agregar 100+ nichos sin crear archivos
# Para generar prompts: usar template_prompts.render_prompt(niche, level, mode)
```

### 7.3 Bridge → Autodiagnostico

```python
# bridge.py solo reporta estado interno, no lee archivos externos
```

### 7.5 Krea2 Official Assets (Comfy-Org/Krea-2)

```
diffusion_models/
  krea2_raw_bf16.safetensors
  krea2_raw_fp8_scaled.safetensors
  krea2_raw_int8_convrot.safetensors
  krea2_turbo_bf16.safetensors
  krea2_turbo_fp8_scaled.safetensors
  krea2_turbo_int8_convrot.safetensors
  krea2_turbo_mxfp8.safetensors
  krea2_turbo_nvfp4.safetensors
text_encoders/
  qwen3vl_4b_bf16.safetensors
  qwen3vl_4b_fp8_scaled.safetensors
vae/
  qwen_image_vae.safetensors
loras/
  krea2_darkbrush.safetensors
  krea2_dotmatrix.safetensors
  krea2_kidsdrawing.safetensors
  krea2_neondrip.safetensors
  krea2_rainywindow.safetensors
  krea2_retroanime.safetensors
  krea2_softwatercolor.safetensors
  krea2_sunsetblur.safetensors
  krea2_turbo_lora_rank_64_bf16.safetensors
  krea2_vintagetarot.safetensors
```

Notas:
- El repo oficial no publica VAE alternativo ni GGUF. Default recomendado: qwen_image_vae.
- Template Krea2 usa CLIPLoader(type="krea2"), cfg=1.0 y ConditioningZeroOut.
- Override opcional via CLI: --krea2-vae krea2/qwen_image_vae.safetensors
- Override opcional via CLI: --krea2-unet krea2/krea2_turbo_fp8_scaled.safetensors
- Override opcional via CLI: --krea2-clip krea2/qwen3vl_4b_fp8_scaled.safetensors
- Wizard en execute: selector interactivo si backend=krea2 y no hay overrides (desactivar con --non-interactive)
- Perfil Krea2: --krea2-profile recommended (atajo no interactivo)

### 7.4 Mode D Level Adjustment

```python
# Modo D (Auto-Pilot) es mas permisivo: +15 al score
# Razon: modo D esta disenado para escalar multi-nicho,
# necesita llegar a niveles mas altos con menor score individual
# Efecto: score 60 → ADVANCED (vs INTERMEDIATE en modo A)
```

---

## 8. FASES COMPLETADAS (v1.0.0)

```
Fase 1 — Autocontencion + Datos (v0.1.0)
  [✓] pyproject.toml con extras_require
  [✓] data/green_grid.py — 120 nichos con 6-factor scoring
  [✓] data/niches.py — 13 templates × 4 niveles
  [✓] data/tools.py — 12 tool guides
  [✓] bridge.py reconvertido a autodiagnostico
  [✓] validator.py con word boundaries + fallback heuristico
  [✓] 4 modos de uso (A/B/C/D) en modes.py

Fase 2 — Niveles + Templates + Tests (v0.2.0)
  [✓] score_to_level() acepta mode_id
  [✓] LEVEL_RANGES por modo (MODE_LEVEL_ADJUSTMENT)
  [✓] 6 stages reciben level consistente
  [✓] CLI --level flag
  [✓] data/template_prompts.py — render_prompt()
  [✓] 45 tests pytest (validator + pipeline + templates)
  [✓] 15 integration tests bash
  [✓] test_integration.sh: ((VAR++)) → VAR=$((VAR + 1)) fix

Fase 3 — Motor de Render (v0.3.0)
  [✓] render/client.py — ComfyUIClient async
  [✓] render/workflows.py — WorkflowBuilder + 4 templates
  [✓] render/engine.py — RenderEngine orquestador
  [✓] RenderStage integrado con engine real
  [✓] scripts/render.sh — CLI directa
  [✓] 19 tests render (64 total pytest)
  [✓] Session cleanup sin warnings "Unclosed client session"

Fase 4 — Analytics (v0.4.0)
  [✓] analytics/revenue_model.py — RevenueForecast con estimaciones
  [✓] analytics/trend_engine.py — TrendProjection 30/60/90d
  [✓] analytics/dashboard.py — DashboardReport + export JSON
  [✓] CLI --analytics flag
  [✓] AnalyticsStage — 7mo stage del pipeline
  [✓] 28 tests analytics

Fase 5 — Multi-cuenta (v0.5.0)
  [✓] accounts/account.py — Account dataclass
  [✓] accounts/manager.py — CRUD con persistencia JSON
  [✓] accounts/rotation.py — RotationEngine con calendario semanal
  [✓] accounts/portfolio.py — PortfolioReport + revenue agregado
  [✓] CLI --accounts / --multi N / --account-add/rm
  [✓] 26 tests accounts

Fase 6 — Web API (v0.6.0)
  [✓] web/api.py — FastAPI con 8 endpoints REST
  [✓] web/server.py — Uvicorn runner
  [✓] CLI --serve / --port / --host
  [✓] Swagger docs en /docs
  [✓] 15 tests web (TestClient)
  [✓] [web] optional dependency group

Fase 7 — Produccion (v0.7.0)
  [✓] Dockerfile multi-stage (python:3.12-slim)
  [✓] docker-compose.yml con healthcheck
  [✓] .dockerignore + .gitignore
  [✓] .github/workflows/test.yml (CI)
  [✓] 133 tests pytest + 15 bash integration

Fase 8 — Render 2026 + Publish + v1.0.0
  [✓] Wan 2.2 workflow template (Modo B, 24GB VRAM, Apache 2.0)
  [✓] LTX-2.3 workflow template (Modo A, 12GB VRAM, video rapido)
  [✓] HunyuanVideo workflow template (Modo C, cinematografico)
  [✓] ReplicateBackend — backend alternativo cloud
  [✓] RenderEngine.backend selector (comfyui|replicate|auto)
  [✓] PublishStage — TikTok, YouTube, Instagram, Reddit
  [✓] CLI --publish / --platform flags
  [✓] .env.template con todas las API keys dummy
  [✓] warmup_plan() en RotationEngine (7-10 dias ramp-up)
  [✓] Apache 2.0 LICENSE + AUTHORS
  [✓] Security audit: 0 shell=True, 0 hardcoded keys
  [✓] 133 tests all passing
```

---

## 9. PROXIMAS FASES (Post-v1.0.0)

### Fase 9 — Dashboard Web UI

```pseudocode
OBJETIVO:
  Interfaz web grafica (HTML+JS) para gestion visual del pipeline.
  Construido sobre la API REST existente.

FRONTEND:
  - Landing page con selector de modo visual
  - Formulario de inyeccion de idea
  - Dashboard de resultados con graficos
  - Explorador de proyectos scaffolded
  - Portfolio view multi-cuenta

STACK:
  - HTML/CSS vanilla o Single SPA
  - Charts.js para visualizacion de revenue
  - Sin framework pesado (sostenible para un solo dev)
```

### Fase 10 — Deploy Multi-Servicio

```pseudocode
OBJETIVO:
  Deploy completo en VPS/EC2 con todos los servicios.

SERVICIOS DOCKER:
  - ai-monetize-api (FastAPI)
  - comfyui (GPU opcional, nvidia-docker)
  - n8n (automatizacion, opcional)
  - postgres (estado persistente, opcional)
  - traefik/nginx (reverse proxy + SSL)

INFRAESTRUCTURA:
  - docker-compose.prod.yml
  - scripts/deploy.sh (CI/CD push-to-deploy)
  - scripts/backup.sh (S3/R2, retencion 7-4-3)
  - scripts/health.sh (checks + alerta webhook)
```

---

## 10. COMANDOS UTILES

```bash
# Dry-run rapido
python3 -m ai_monetize --mode A --niche "fitness yoga" --dry-run

# Solo imagenes (sin video)
python3 -m ai_monetize --mode A --niche "fitness" --content-type images --dry-run

# Solo video
python3 -m ai_monetize --mode A --niche "fitness" --content-type videos --dry-run

# Templates legacy SDXL (v1) vs 2026 Wan/LTX (v2)
python3 -m ai_monetize --mode A --niche "fitness" --template-version v1 --dry-run
python3 -m ai_monetize --mode A --niche "fitness" --template-version v2 --dry-run

# Backend cloud via Replicate
python3 -m ai_monetize --mode D --niche "multi-niche" --backend replicate --dry-run

# Ejecutar con scaffolding
python3 -m ai_monetize --mode B --niche "cosplay anime" --execute

# Publicar en plataformas
python3 -m ai_monetize --publish --niche fitness --mode A
python3 -m ai_monetize --publish --niche fitness --platform tiktok youtube

# Override de nivel
python3 -m ai_monetize --mode D --niche "multi-niche" --level expert --dry-run

# Solo validar Green Grid
python3 -m ai_monetize --validate "finanzas personales"

# Dashboard analytics
python3 -m ai_monetize --analytics "ai tools" --mode B
python3 -m ai_monetize --analytics "fitness" --level advanced

# Portfolio multi-cuenta
python3 -m ai_monetize --multi 5 --niche "fitness" --mode D
python3 -m ai_monetize --accounts
python3 -m ai_monetize --account-add "mi_cuenta" --niche "fitness" --mode A

# Warm-up plan
python3 -c "from ai_monetize.accounts.rotation import RotationEngine; r=RotationEngine(); print(r.warmup_plan(7))"

# Servidor web API
python3 -m ai_monetize --serve
python3 -m ai_monetize --serve --port 8080 --host 127.0.0.1

# Render directo
bash scripts/render.sh --mode A --niche "fitness" --dry-run
bash scripts/render.sh --mode B --niche "cosplay" --level advanced --execute

# Tests
python3 -m pytest tests/ -v               # 133 tests
bash scripts/test_integration.sh          # 15 smoke tests

# Docker
docker compose up -d --build
curl http://localhost:8000/docs

# Bridge status (autodiagnostico)
python3 -m ai_monetize --bridge
```

---

## 11. DEPENDENCIAS

```python
# Minimo (zero): Python 3.12+ standard library only
# Sin pip install necesario para el core

# pip install ai-monetize[web]       — Servidor API REST
#   fastapi>=0.110       — Framework web
#   uvicorn[standard]>=0.29 — ASGI server
#   pydantic>=2.5        — Validacion de datos

# pip install ai-monetize[render]    — Motor ComfyUI
#   aiohttp>=3.9         — HTTP async para ComfyUI client
#   pillow>=10.0         — Procesamiento de imagenes

# pip install ai-monetize[full]      — Todo incluido
#   aiohttp>=3.9, pillow>=10.0
#   fastapi>=0.110, uvicorn>=0.29, pydantic>=2.5
#   requests>=2.31

# pip install ai-monetize[dev]       — Desarrollo
#   pytest>=7            — Testing framework
#   pytest-asyncio       — Soporte async tests
```

---

## 12. ESTRUCTURA DE ARCHIVOS COMPLETA (v1.0.0)

```
/opt/ai-monetize/
├── ai_monetize/               # Nucleo Python
│   ├── __init__.py            # version 1.0.0, exports globales
│   ├── __main__.py            # python -m ai_monetize entry
│   ├── cli.py                 # argparse + 17 flags + menu + --publish
│   ├── engine.py              # PipelineEngine, Idea, PipelineResult
│   ├── stages/                # 8 Stage classes modulares
│   │   ├── __init__.py        # STAGES list ordenada
│   │   ├── registry.py        # StageID, Stage, StageResult, COST_PER_MODE
│   │   ├── validate.py        # ValidateStage
│   │   ├── plan.py            # PlanStage
│   │   ├── create.py          # CreateStage
│   │   ├── render.py          # RenderStage
│   │   ├── distribute.py      # DistributeStage (estrategia)
│   │   ├── publish.py         # PublishStage (ejecucion TikTok/YT/IG/Reddit)
│   │   ├── monetize.py        # MonetizeStage
│   │   └── analytics.py       # AnalyticsStage
│   ├── modes.py               # 4 Mode dataclasses frozen
│   ├── validator.py           # Green Grid validate() + format_validation()
│   ├── bridge.py              # Autodiagnostico
│   │
│   ├── models/                # Dataclasses
│   │   ├── niche.py           # Level enum, score_to_level
│   │   ├── character.py       # CharacterBible
│   │   └── project.py         # ProjectScaffold
│   │
│   ├── data/                  # Datos embebidos (zero file I/O)
│   │   ├── green_grid.py      # 120 GridEntry con 6-factor scoring
│   │   ├── niches.py          # 13 NicheTemplate × 4 niveles
│   │   ├── template_prompts.py  # render_prompt(niche, level, mode)
│   │   └── tools.py           # 12 ToolGuide
│   │
│   ├── render/                # Motor async
│   │   ├── client.py          # ComfyUIClient + ReplicateBackend
│   │   ├── workflows.py       # WorkflowBuilder + v1 + v2 templates
│   │   └── engine.py          # RenderEngine con backend selector
│   │
│   ├── analytics/             # Revenue + tendencias
│   │   ├── revenue_model.py   # RevenueForecast
│   │   ├── trend_engine.py    # TrendProjection 30/60/90d
│   │   └── dashboard.py       # DashboardReport + export JSON
│   │
│   ├── accounts/              # Multi-cuenta
│   │   ├── account.py         # Account dataclass
│   │   ├── manager.py         # CRUD + ~/.ai-monetize/accounts.json
│   │   ├── rotation.py        # RotationEngine + warmup_plan()
│   │   └── portfolio.py       # PortfolioReport agregado
│   │
│   └── web/                   # FastAPI REST
│       ├── api.py             # 8 endpoints
│       └── server.py          # Uvicorn runner
│
├── tests/                     # 133 tests pytest + 15 bash
│   ├── test_validator.py      # 23 tests
│   ├── test_pipeline.py       # 11 tests
│   ├── test_templates.py      # 11 tests
│   ├── test_render.py         # 19 tests
│   ├── test_analytics.py      # 28 tests
│   ├── test_accounts.py       # 26 tests
│   └── test_web.py            # 15 tests
│
├── scripts/
│   ├── test_integration.sh    # 15 smoke tests bash
│   ├── setup.sh               # Crea venv/ autocontenido
│   ├── render.sh              # CLI render directo
│   └── validate_structure.sh  # Validacion de estructura
│
├── projects/                  # Scaffolded projects (gitignored)
├── docs/
│   ├── TECHNICAL.md           # Blueprint tecnico (este archivo)
│   ├── CHANGELOG.md           # Historial completo v0.1.0 → v0.7.0
│   ├── GUIDE_WALKTHROUGH.md   # Guia de usuario
│   └── MASTER_PLAN.md         # Plan maestro original
│
├── .github/workflows/
│   └── test.yml               # CI: pytest + integration + imports
│
├── Dockerfile                 # Multi-stage build (python:3.12-slim)
├── docker-compose.yml         # API + healthcheck + volumen
├── .dockerignore              # Excluye tests, venv, docs
├── .gitignore                 # Excluye cache, env, projects
├── pyproject.toml             # v1.0.0, 4 extra groups, Apache 2.0
├── Makefile                   # Comandos comunes
├── README.md                  # Documentacion de usuario
├── CONFIG.md                  # Guia de configuracion y .env
├── .env.template              # Template de variables de entorno
└── QUICKSTART.md              # Guia rapida 2-min
```

---

## 13. REGLAS PARA FUTURAS MODIFICACIONES

1. **Zero external deps runtime**: El core debe funcionar con `python3` sola.
   Los extras son para funcionalidad aumentada (render, analytics).

2. **No hardcodear API keys**: Siempre `os.environ.get()` o `.env`.
   Nunca en codigo, nunca en git.

3. **No shell=True en subprocess**: Siempre `asyncio.create_subprocess_exec(*args)`.
   Ver `AGENTS.md` regla 1.

4. **Templates como datos, no archivos**: Agregar un nicho = agregar un `_reg()` en
   `data/niches.py`. No crear carpetas, no crear markdowns.

5. **Level-aware siempre**: Toda funcionalidad nueva debe aceptar `level` y
   comportarse distinto segun nivel. Es el eje central del proyecto.

6. **Dry-run primero**: Toda fase nueva debe funcionar en dry-run sin tocar
   APIs externas. El execute es el dry-run + side effects.

7. **Test coverage**: Cada fase nueva debe incluir tests pytest + verificar
   que `bash scripts/test_integration.sh` sigue pasando.

8. **Idempotencia**: Ejecutar el mismo pipeline dos veces debe dar el mismo
   resultado (para el mismo seed/project_dir).

---

## 14. GLOSARIO

```
Green Grid:     Matriz de 120 nichos con 6 factores de scoring (0-100)
Level:          BEGINNER < INTERMEDIATE < ADVANCED < EXPERT
Mode:           A (Faceless) | B (Character) | C (Cine) | D (Auto-Pilot)
Stage:          VALIDATE → PLAN → CREATE → RENDER → DISTRIBUTE → PUBLISH → MONETIZE → ANALYTICS
PublishStage:   Stage de ejecucion que publica assets en TikTok, YouTube, Instagram, Reddit
                Simulado sin API keys, real con keys en .env
Dry-run:        Validacion sin gastar tokens ni generar media
Execute:        Ejecucion real con scaffolding de proyecto
Scaffold:       Creacion de estructura de proyecto en disco
Workflow:       JSON de nodos ComfyUI (WanVideoLoader, HunyuanVideoLoader, etc.)
Prompt:         Template de texto para generacion de contenido por nivel
Score:          Ponderacion 6 factores: Trend, Monetization, Competition,
                Repeatability, Faceless, Beginner
RevenueForecast: Estimacion de ingresos por nivel/nicho/modo (3 periodos)
TrendProjection: Proyeccion 30/60/90 dias basada en factores Green Grid + estacionalidad
Account:        Perfil multi-plataforma con nombre, nicho, modo, nivel, estado
Portfolio:      Conjunto de cuentas con revenue agregado y metricas
Warm-up:        Ramp-up gradual de 7-10 dias para cuentas nuevas (RotationEngine.warmup_plan)
Replicate:      Backend cloud alternativo a ComfyUI via Replicate API (Wan 2.2, LTX-2.3)
Backend:        Selector en RenderEngine: comfyui | replicate | auto
Wan 2.2:        Modelo video 2024/2026 de Alibaba, 24GB VRAM, Apache 2.0, Modo B
LTX-2.3:        Modelo video rapido Lightricks, 12GB VRAM, bajo costo, Modo A
HunyuanVideo:   Modelo video cinematografico Tencent, A100/H100, Modo C
Word boundary:  \b en regex para evitar falsos positivos ("ai" en "daily")
```
