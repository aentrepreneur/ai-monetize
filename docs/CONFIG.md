# CONFIG: AI-Monetize v1.0.0

## .env Template

Copia `.env.template` a `.env`:

```bash
cp .env.template .env
# Edita con tus API keys
```

## Variables de Entorno

### LLM

| Variable | Default | Descripcion |
|----------|---------|-------------|
| `LLM_API_KEY` | — | API key del proveedor LLM |
| `LLM_PROVIDER` | `groq` | `groq` / `openrouter` / `gemini` / `openai` |

### Render Backends

| Variable | Default | Descripcion |
|----------|---------|-------------|
| `COMFYUI_BASE_URL` | `http://127.0.0.1:8188` | URL del servidor ComfyUI |
| `REPLICATE_API_KEY` | — | API key de Replicate.com |
| `REPLICATE_MODEL_WAN` | `cuda-labs/wan-video` | Modelo Wan 2.2 en Replicate |
| `REPLICATE_MODEL_LTX` | `lightricks/ltx-2.3` | Modelo LTX-2.3 en Replicate |

### Krea2 (ComfyUI)

Modelos oficiales en Hugging Face (Comfy-Org/Krea-2). Se recomienda:

```
diffusion_models/krea2/krea2_turbo_fp8_scaled.safetensors
text_encoders/krea2/qwen3vl_4b_fp8_scaled.safetensors
vae/krea2/qwen_image_vae.safetensors
```

Script recomendado:

```bash
bash scripts/setup_comfyui_krea2.sh 20 --variant turbo
```

Override opcional de VAE:

```bash
python3 -m ai_monetize --mode A --niche "fitness" --backend krea2 --execute \
  --krea2-vae krea2/qwen_image_vae.safetensors
```

Override opcional de UNet/CLIP:

```bash
python3 -m ai_monetize --mode A --niche "fitness" --backend krea2 --execute \
  --krea2-unet krea2/krea2_turbo_fp8_scaled.safetensors \
  --krea2-clip krea2/qwen3vl_4b_fp8_scaled.safetensors
```

### Publishing Platforms

| Variable | Descripcion |
|----------|-------------|
| `TIKTOK_API_KEY` | TikTok Open API key |
| `YOUTUBE_API_KEY` | YouTube Data API key |
| `INSTAGRAM_API_KEY` | Instagram Graph API key |
| `REDDIT_API_KEY` | Reddit API key |
| `REDDIT_USERNAME` | Usuario Reddit |
| `REDDIT_PASSWORD` | Password Reddit |

### General

| Variable | Default | Descripcion |
|----------|---------|-------------|
| `LOG_LEVEL` | `INFO` | `DEBUG` / `INFO` / `WARNING` / `ERROR` |
| `TIMEZONE` | `America/Argentina/Buenos_Aires` | Zona horaria |

## Backend Selection

El `RenderEngine` acepta `backend` parameter:

```python
RenderEngine(backend="auto")       # Segun workflow template (default)
RenderEngine(backend="comfyui")    # Solo ComfyUI local
RenderEngine(backend="krea2")      # ComfyUI con templates Krea2
RenderEngine(backend="replicate")  # Solo Replicate cloud
```

En CLI hay flag `--backend`.

## Content Type

Controla que tipo de assets generar:

| Flag | Efecto |
|------|--------|
| `--content-type all` | Imagenes + videos (default) |
| `--content-type images` | Solo imagenes |
| `--content-type videos` | Solo videos |

```bash
python3 -m ai-monetize --mode A --niche fitness --content-type images --dry-run
```

## Template Version

Elige entre workflows legacy (v1, SDXL/AnimateDiff) o 2026 (v2, Wan/LTX/Hunyuan):

| Flag | Templates |
|------|-----------|
| `--template-version v1` | SDXL, IPAdapter, AnimateDiff (legacy) |
| `--template-version v2` | Wan 2.2, LTX-2.3, HunyuanVideo (default) |

## Backend

| Flag | Backend |
|------|---------|
| `--backend comfyui` | Servidor ComfyUI local |
| `--backend krea2` | ComfyUI con templates Krea2 |
| `--backend replicate` | Replicate API cloud |
| `--backend auto` | Segun template (default) |

## Overrides Krea2

| Flag | Descripcion |
|------|-------------|
| `--krea2-vae` | Override del VAE Krea2 (default: qwen_image_vae) |
| `--krea2-unet` | Override del UNet Krea2 (default: krea2_turbo_fp8_scaled) |
| `--krea2-clip` | Override del CLIP Krea2 (default: qwen3vl_4b_fp8_scaled) |
| `--krea2-profile` | Perfil Krea2 (recommended, turbo_fp8, turbo_bf16, raw_*) |
| `--non-interactive` | Desactiva wizard en execute |

## Niveles

| Level | Score Range | Ajuste por Modo | Render Scale |
|-------|-------------|-----------------|-------------|
| BEGINNER | 0-59 | A: +0, B: -5, C: -5, D: +15 | 10 img + 5 vid |
| INTERMEDIATE | 60-74 | igual | 25 img + 10 vid |
| ADVANCED | 75-84 | igual | 50 img + 20 vid |
| EXPERT | 85-100 | igual | 100 img + 40 vid |

## Publicacion (PublishStage)

Cada plataforma requiere API key en `.env`. Sin keys, funciona en modo simulado:

```bash
# Dry-run (simulado)
python3 -m ai_monetize --publish --niche fitness --mode A

# Publicar en plataformas especificas
python3 -m ai_monetize --publish --niche fitness --platform tiktok youtube instagram
```

## Warm-up Plan

El `RotationEngine.warmup_plan(days=7)` genera schedule de ramp-up gradual para cuentas nuevas:

```python
from ai_monetize.accounts.rotation import RotationEngine
from ai_monetize.accounts.manager import AccountManager

mgr = AccountManager()
rot = RotationEngine(mgr.list_accounts("active"))
plan = rot.warmup_plan(7)
print(plan)
```
