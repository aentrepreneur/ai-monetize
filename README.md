<div align="center">

# AI-Monetize: De la Idea al Ingreso

[![Version](https://img.shields.io/badge/version-1.0.1-blue)](https://github.com/angesquivel/ai-monetize)
[![Python](https://img.shields.io/badge/python-3.12%2B-blue)](https://python.org)
[![License](https://img.shields.io/badge/license-Apache%202.0-green)](LICENSE)
[![Tests](https://img.shields.io/badge/tests-133%2F133-brightgreen)](tests/)
[![Last Update](https://img.shields.io/badge/updated-2026--07--09-blue)](https://github.com/angesquivel/ai-monetize)

Pipeline completo de monetizacion con IA: valida un nicho en **0.1s sin gastar tokens**, genera plan de ejecucion, renderiza assets con Wan 2.2/LTX-2.3/HunyuanVideo via ComfyUI o Replicate, publica en plataformas, analiza revenue, gestiona multi-cuenta, y sirve API REST.

```bash
python3 -m ai_monetize --mode A --niche "fitness yoga" --dry-run
```

</div>

---

## 📦 Pipeline (8 stages)

```
VALIDATE → PLAN → CREATE → RENDER → DISTRIBUTE → PUBLISH → MONETIZE → ANALYTICS
```

Cada stage funciona en dry-run (solo texto, $0) o execute (genera assets reales).

---

## 🎮 Modos

| Modo | Nombre | Costo/mes | Riesgo | Timeline $5k | Motor Render |
|------|--------|-----------|--------|--------------|-------------|
| A | Faceless Channels | $0-60 | Bajo | Mes 4-6 | LTX-2.3 (12GB VRAM) |
| B | AI Character | $0-30 | Medio | Mes 4-6 | Wan 2.2 (24GB VRAM, Apache 2.0) |
| C | Cine Fusion | $20-100 | Medio-Alto | Mes 3-5 | HunyuanVideo (cine, A100+) |
| D | Auto-Pilot | $40-180 | Alto | Mes 2-4 | Wan batch / Replicate API |

---

## 🚀 Quick Start

> [!TIP]
> Todo funciona en **dry-run** sin gastar un solo token de API. El pipeline completo se ejecuta local con zero costos externos.

```bash
# Validar nicho (GRATIS, 0.1s)
python3 -m ai_monetize --validate "fitness yoga"

# Pipeline dry-run (sin gastar un token)
python3 -m ai_monetize --mode A --niche "fitness yoga" --dry-run

# Ejecucion real (scaffold proyecto)
python3 -m ai_monetize --mode B --niche "cosplay anime" --execute

# Publicar en plataformas (dry-run)
python3 -m ai_monetize --publish --niche "fitness" --mode A
python3 -m ai_monetize --publish --niche "fitness" --platform tiktok youtube

# Dashboard analytics
python3 -m ai_monetize --analytics "ai tools" --mode B

# Portfolio multi-cuenta
python3 -m ai_monetize --multi 5 --niche "fitness" --mode D
python3 -m ai_monetize --accounts

# Warm-up plan
python3 -c "from ai_monetize.accounts.rotation import RotationEngine; r=RotationEngine(); print(r.warmup_plan(7))"

# Servidor web API
python3 -m ai_monetize --serve
# → http://localhost:8000/docs
```

---

## ⚙️ CLI Flags

| Flag | Descripcion |
|------|-------------|
| `--menu` | Menu interactivo |
| `--mode A/B/C/D` | Modo de uso |
| `--niche "texto"` | Nicho a procesar |
| `--level` | Override de nivel |
| `--dry-run` / `--execute` | Modo simulacion o real |
| `--validate "texto"` | Solo Green Grid |
| `--analytics [niche]` | Dashboard revenue + tendencias |
| `--publish` | Publicar en plataformas |
| `--platform tiktok youtube ...` | Plataformas destino |
| `--content-type all/images/videos` | Tipo de contenido a generar |
| `--template-version v1/v2` | Version de templates de render |
| `--backend comfyui/replicate/auto` | Backend de render |
| `--krea2-vae` | Override del VAE Krea2 (default: qwen_image_vae) |
| `--krea2-unet` | Override del UNet Krea2 (default: krea2_turbo_fp8_scaled) |
| `--krea2-clip` | Override del CLIP Krea2 (default: qwen3vl_4b_fp8_scaled) |
 | `--krea2-profile` | Perfil Krea2 (recommended, turbo_fp8, turbo_bf16, raw_*) |
 | `--krea2-enhancer-strength` | Strength Krea2T-Enhancer 0-2 (0=desactivado) |
 | `--krea2-text-scale` | text_scale Krea2T Enhancer Advanced (1.5-2.0) |
 | `--non-interactive` | Desactiva wizard en execute |
| `--accounts` | Portfolio multi-cuenta |
| `--multi N` | Crear N cuentas batch |
| `--account-add/rm` | CRUD cuentas individuales |
| `--cycles N` | Ciclos de profit compuesto |
| `--reinvest 0.7` | Tasa de reinversion |
| `--serve` / `--port` / `--host` | Servidor web API |
| `--bridge` | Estado del sistema |

---

## 🏗️ Arquitectura

<details>
<summary>Click para expandir arbol de directorios</summary>

```
ai-monetize/
├── ai_monetize/          # Nucleo Python
│   ├── cli.py            # argparse + menu interactivo
│   ├── engine.py         # PipelineEngine orquestador
│   ├── stages/           # 8 Stage classes modulares
│   │   ├── registry.py   # StageID, Stage, StageResult
│   │   ├── publish.py    # PublishStage (TikTok/YT/IG/Reddit)
│   │   └── ...
│   ├── modes.py          # 4 Modos A/B/C/D
│   ├── validator.py      # Green Grid (120 nichos, 6 factores)
│   │
│   ├── data/             # Datos embebidos
│   ├── models/           # Dataclasses
│   ├── render/           # ComfyUIClient + ReplicateBackend + WorkflowBuilder
│   │   ├── client.py     # ComfyUIClient + ReplicateBackend async
│   │   ├── workflows.py  # MODE_WORKFLOWS v1 + MODE_WORKFLOWS_V2
│   │   └── engine.py     # RenderEngine con backend selector
│   ├── analytics/        # Revenue + trend projection
│   ├── accounts/         # Multi-cuenta + rotation + warmup
│   └── web/              # FastAPI REST server
│
├── tests/                # 133 tests pytest
├── scripts/              # test_integration, setup, render
├── docs/                 # TECHNICAL, CHANGELOG, CONFIG
├── Dockerfile            # Multi-stage build (python:3.12-slim)
├── docker-compose.yml    # API + healthcheck + volumen
└── .github/workflows/    # CI (pytest + integration)
```

</details>

---

## 🎨 Render Backends

| Backend | Uso | Configuracion |
|---------|-----|---------------|
| **ComfyUI** | Local (GPU) | `COMFYUI_BASE_URL` en `.env` |
| **Replicate** | Cloud (API) | `REPLICATE_API_KEY` en `.env` |
| **Auto** | Segun template | Detecta `backend` field en workflow |

---

## 🖼️ Krea2 (ComfyUI) Quick Setup

> [!IMPORTANT]
> Requiere ComfyUI v0.26+ con GPU NVIDIA y CUDA 12.x. Modelos oficiales del repo [Comfy-Org/Krea-2](https://huggingface.co/Comfy-Org/Krea-2).

Modelos oficiales (Comfy-Org/Krea-2):

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

Instalacion automatica:

```bash
bash scripts/setup_comfyui_krea2.sh 20 --variant turbo
```

Notas:
- El repo oficial no publica VAE alternativo ni GGUF. Se documenta solo Qwen image VAE.
- Si ComfyUI no detecta modelos, presiona `R` en el canvas para refrescar.
- Si Sage Attention produce imagenes negras, inicia ComfyUI sin Sage.

> [!TIP]
> **Atajos de perfil**: `--krea2-profile recommended` evita el wizard.
> **Prompt adherence**: Krea2T-Enhancer disponible con `--krea2-enhancer-strength 1.5`.

- Override de VAE: `--krea2-vae krea2/qwen_image_vae.safetensors`
- Override de UNet: `--krea2-unet krea2/krea2_turbo_fp8_scaled.safetensors`
- Override de CLIP: `--krea2-clip krea2/qwen3vl_4b_fp8_scaled.safetensors`
- Wizard en execute: si `--backend krea2` y no hay overrides, se abre selector interactivo.
- Perfil Krea2: `--krea2-profile recommended` (evita wizard)
- Krea2T-Enhancer prompt adherence: `--krea2-enhancer-strength 1.5 --krea2-text-scale 1.8`
- Instalar enhancer en setup: `./scripts/setup_comfyui_krea2.sh --enhancer`

---

## 🧪 Tests

```bash
python3 -m pytest tests/ -v           # 133 tests (0.5s)
bash scripts/test_integration.sh      # 15 smoke tests
```

---

## 🐳 Docker

```bash
docker compose up -d --build
# → http://localhost:8000
# → http://localhost:8000/docs
```

---

## 📋 Dependencias

Core: **Python 3.12+ stdlib only** (zero deps).

| Extras | Instalacion | Incluye |
|--------|------------|---------|
| `[web]` | `pip install ai-monetize[web]` | fastapi, uvicorn, pydantic |
| `[render]` | `pip install ai-monetize[render]` | aiohttp, pillow |
| `[full]` | `pip install ai-monetize[full]` | Todo lo anterior |

---

## 📜 Licencia

Apache 2.0 — D1 Open Core. Ver `LICENSE` y `AUTHORS`.
