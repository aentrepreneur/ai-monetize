# GUIA COMPLETA: AI-Monetize — De la Idea al Ingreso

Bienvenido. Esta guia te lleva desde cero hasta dominar el pipeline completo
de monetizacion con contenido generado por IA usando `ai-monetize`, un sistema
unificado que integra validacion de nichos, planificacion, generacion de
prompts y orquestacion de render en un solo CLI.

```
Autor: Angel Esquivel (CyberSecurity)
Version: 0.1.0 — Julio 2026
Stack: Python 3.12+ | Sin dependencias externas
```

---

## Indice

1. [Filosofia del Sistema](#1-filosofia-del-sistema)
2. [Arquitectura General](#2-arquitectura-general)
3. [El Proyecto Unificado](#3-el-proyecto-unificado)
4. [Instalacion y Primeros Pasos](#4-instalacion-y-primeros-pasos)
5. [Modos de Uso Operativos](#5-modos-de-uso-operativos)
6. [Pipeline Detallado: 6 Etapas](#6-pipeline-detallado-6-etapas)
7. [Dry-Run: Validacion Sin Costo](#7-dry-run-validacion-sin-costo)
8. [Ejemplo Practico: Fitness Yoga Mujeres 40+](#8-ejemplo-practico-fitness-yoga-mujeres-40)
9. [Ejemplo Practico: AI Character Cosplay Anime](#9-ejemplo-practico-ai-character-cosplay-anime)
10. [Estructura de Proyecto Generado](#10-estructura-de-proyecto-generado)
11. [Monetizacion en Cascada](#11-monetizacion-en-cascada)
12. [Roadmap y Proximos Pasos](#12-roadmap-y-proximos-pasos)
13. [Preguntas Frecuentes](#13-preguntas-frecuentes)

---

## 1. Filosofia del Sistema

### Dry-First, Execute Second

La regla de oro de AI-Monetize:

> "Dry-run primero, ejecuta despues. No gastes tokens en ideas que no usaras."

Cada etapa del pipeline se puede ejecutar en modo simulacion.
Solo cuando validas que la idea es viable de principio a fin,
liberas la generacion real de media (imagenes, video).

### Sistema Unificado

No se trata de prompts sueltos. Se trata de un SISTEMA.
Las 6 etapas estan conectadas: la salida de una alimenta la entrada de la siguiente.

| ai-monetize |
|---|
| Maquina |
| Orquestacion |
| Pipeline + Dry-Run |
| 6 etapas unificadas |

---

## 2. Arquitectura General

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        AI-MONETIZE ENGINE                                │
│                                                                          │
│  ┌──────────────────────────────────────────────────────────────┐       │
│  │                     PIPELINE (6 ETAPAS)                       │       │
│  │                                                               │       │
│  │  VALIDATE ──▶ PLAN ──▶ CREATE ──▶ RENDER ──▶ DISTRIBUTE ──▶ MONETIZE  │
│  │     │           │          │          │            │            │     │
│  │     ▼           ▼          ▼          ▼            ▼            ▼     │
│  │  Green Grid  Stack +   Prompts   Imagenes +  TikTok, IG,   Passes    │
│  │  Score       Budget    Templates  Video       Reddit       + RM11    │
│  │                                                               │       │
│  │  Cada etapa: [dry-run → solo texto] [execute → genera media]  │       │
│  └──────────────────────────────────────────────────────────────┘       │
└─────────────────────────────────────────────────────────────────────────┘
```

### Flujo de Datos

```
Entrada: "fitness yoga mujeres 40+"

VALIDATE → GREEN (88/100) → procede
    │
PLAN → Modo A Faceless, $60-100/mes, timeline 4-6 meses
    │
CREATE → 13 templates de prompts generados
    │
RENDER → [DRY-RUN: SKIP] $15.45 ahorrados en APIs
    │
DISTRIBUTE → TikTok > Instagram > YouTube > Reddit
    │
MONETIZE → Passes + RM11 + productos digitales → estimado $5k mes 4-6
```

---

## 3. El Proyecto Unificado

### ai-monetize (`/opt/ai-monetize/`)

**Proposito:** Sistema unificado que cubre todo el pipeline: validacion, planificacion,
generacion de prompts, render, distribucion y monetizacion. Sin dependencias externas.

Contiene:
- CLI con menu interactivo (4 modos + inyector de ideas)
- Pipeline de 8 etapas con dry-run
- Validador Green Grid embebido (120 nichos)
- Templates de nicho por nivel (13 × 4 niveles)
- Motor de render para ComfyUI + Replicate
- Project scaffolding en `projects/`

**Usalo cuando:** Quieras procesar una idea completa desde la validacion
hasta el plan de monetizacion, sin gastar tokens en media no validada.

---

## 4. Instalacion y Primeros Pasos

### Requisitos

```bash
Python 3.12+
# Sin dependencias externas (solo stdlib)
```

### Verificar Instalacion

```bash
cd /opt/ai-monetize
python3 -m ai_monetize --help
# Debe mostrar el menu de ayuda con flags
```

### Verificar Estado

```bash
python3 -m ai_monetize --bridge
# Muestra el estado interno de los modulos embebidos
```

### Primer Comando

```bash
# Valida un nicho SIN gastar nada
python3 -m ai_monetize --validate "fitness yoga"
```

### Menu Interactivo

```bash
python3 -m ai_monetize --menu
# Navega por modos, inyecta ideas, valida nichos
```

### Krea2 (ComfyUI) Setup

```bash
# Instalacion automatica
bash scripts/setup_comfyui_krea2.sh 20 --variant turbo

# Luego en .env
COMFYUI_BASE_URL=http://<IP_CLOUD>:8188
```

Notas:
- Si ComfyUI no detecta modelos, presiona `R` en el canvas para refrescar.
- Krea2 turbo usa steps=8 y cfg=1.0 (no negative prompt).
- Override VAE: `--krea2-vae krea2/qwen_image_vae.safetensors`
- Override UNet: `--krea2-unet krea2/krea2_turbo_fp8_scaled.safetensors`
- Override CLIP: `--krea2-clip krea2/qwen3vl_4b_fp8_scaled.safetensors`
- Wizard en execute: si `--backend krea2` y no hay overrides, se abre selector interactivo.
- Perfil Krea2: `--krea2-profile recommended` (evita wizard)

---

## 5. Modos de Uso Operativos

### Modo A: Faceless Channels

| Aspecto | Detalle |
|---------|---------|
| Descripcion | Contenido educativo/entretenimiento SIN personaje |
| Stack | MoneyPrinterTurbo + Claude/Groq + Pexels + TTS |
| Costo | $0-100/mes |
| Timeline a $5k | 4-6 meses |
| Riesgo | Bajo |
| Ideal para | Escala multi-nicho, minima inversion |

**Cuando usar:** Tienes una idea de contenido que no necesita un personaje
visual. Ej: "Top 5 datos curiosos", "Historia de..." , "Como funciona X".

**Ejemplo:** Canal de finanzas personales con videos faceless de 60 segundos.

### Modo B: AI Character

| Aspecto | Detalle |
|---------|---------|
| Descripcion | Personaje visual consistente con IA |
| Stack | KREA 2 + ComfyUI + Kaggle + LoRA + Passes |
| Costo | $0-20/mes |
| Timeline a $5k | 4-6 meses |
| Riesgo | Medio |
| Ideal para | Marca personal AI, alto engagement |

**Cuando usar:** Quieres crear un personaje AI con identidad visual unica.
Ej: AI influencer de fitness, cosplay, lifestyle.

**Ejemplo:** Chica AI que comparte rutinas de yoga en Instagram y TikTok.

### Modo C: Cine Fusion

| Aspecto | Detalle |
|---------|---------|
| Descripcion | Calidad cinematografica premium |
| Stack | Claude Skills + Wan2GP + KREA 2 + Seedance + DaVinci |
| Costo | $20-86/mes |
| Timeline a $5k | 3-5 meses |
| Riesgo | Medio-Alto |
| Ideal para | Contenido viral, calidad maxima |

**Cuando usar:** Necesitas calidad cinematografica, con direccion de camara
real, lentes anamorficos, color grading profesional.

**Ejemplo:** Cortometrajes AI, videoclips musicales, contenido premium.

### Modo D: Auto-Pilot

| Aspecto | Detalle |
|---------|---------|
| Descripcion | Automatizacion total a escala (10+ cuentas) |
| Stack | N8N + ComfyUI batch + Postiz + Google Sheets |
| Costo | $40-200/mes |
| Timeline a $5k | 2-4 meses |
| Riesgo | Alto |
| Ideal para | Agencias, escalar multiples nichos |

**Cuando usar:** Ya tienes un sistema funcionando y quieres escalar
a 10+ cuentas en diferentes nichos con automatizacion total.

**Ejemplo:** Red de 10 canales faceless de diferentes nichos operando
en auto-pilot con supervisión minima.

---

## 6. Pipeline Detallado: 6 Etapas

### Stage 1: VALIDATE

**Que hace:** Evalua el nicho contra el Green Grid (6 factores).

**Factores:**
- Trend Growth (25%): Crecimiento de views en ultimos 12 meses
- Monetization (25%): Cantidad de income streams disponibles
- Competition (20%): Inverso: menos competencia = mejor
- Repeatability (15%): Capacidad de producir 100+ videos distintos
- Faceless Friendly (10%): Compatibilidad con produccion sin rostro
- Beginner Friendly (5%): Facilidad para empezar

**Scoring:**
- 75-100: GREEN → Proceder
- 50-74: YELLOW → Investigar mas
- 0-49: RED → Evitar

**Modo dry-run:** Scoring completo, 0 gasto.

### Stage 2: PLAN

**Que hace:** Crea el plan de ejecucion completo.

**Output:**
- Stack de herramientas seleccionado
- Budget mensual estimado
- Timeline a $5k/mes
- Content calendar semanal (15 posts)
- Plataformas objetivo

**Modo dry-run:** Plan completo, 0 gasto.

### Stage 3: CREATE

**Que hace:** Genera templates, prompts, scripts.

**Output:**
- Prompts base por modo (13-50 templates)
- Character sheets (si modo B o C)
- Scripts de contenido
- Estructura de proyecto

**Modo dry-run:** SKIP. Muestra templates disponibles sin generar.
Costo evitado: $0.50-$8.00 segun modo.

### Stage 4: RENDER

**Que hace:** Genera imagenes y videos.

**Output:**
- 25 imagenes estimadas
- 10 videos estimados
- Post-procesado (upscale, color)

**Modo dry-run:** SKIP COMPLETO. Sin generacion de media.
Costo evitado: $0.45-$2.40 segun modo.
Tokens ahorrados: ~5,000-20,000.

### Stage 5: DISTRIBUTE

**Que hace:** Estrategia de publicacion y crecimiento.

**Output:**
- Plataformas priorizadas
- Hashtags recomendados
- Posting schedule
- Tactic de warm-up

**Modo dry-run:** Completo, 0 gasto.

### Stage 6: MONETIZE

**Que hace:** Revenue model, pricing, productos.

**Output:**
- Plataforma principal (Passes 10%)
- Pricing tiers ($9.99-$49.99)
- Productos digitales (prompts, presets, cursos)
- Income estimates por mes

**Modo dry-run:** Completo, 0 gasto.

---

## 7. Dry-Run: Validacion Sin Costo

### Que es Dry-Run

Ejecuta las 6 etapas del pipeline en modo simulacion.
Las etapas CREATE y RENDER se saltan completamente.
Las otras 4 etapas se ejecutan con datos reales.

### Que Ahorras

| Recurso | Sin Dry-Run | Con Dry-Run |
|---------|-------------|-------------|
| Costo APIs generacion | $2-30 por ejecucion | $0 |
| Tokens LLM en media | 5,000-30,000 | 0 |
| Tiempo de render | 15-45 minutos | 0 |
| Imagenes generadas | ~25 por batch | 0 |
| Videos generados | ~10 por batch | 0 |

### Como Usarlo

```bash
# Dry-run basico
python3 -m ai_monetize --mode A --niche "fitness yoga" --dry-run

# Dry-run especifico con concepto
python3 -m ai_monetize --mode B --niche "cosplay anime" --concept "personaje gothic lolita" --dry-run
```

### Interpretar Resultados

```
RESUMEN DRY-RUN: fitness yoga | Modo [A] Faceless Channels
══════════════════════════════════════════════════════════
• Tokens ahorrados: ~32,500
• Costo evitado: $15.45
• Tiempo ahorrado: ~35 min
• Viabilidad: ALTA (GREEN)
```

- **Viabilidad ALTA + GREEN**: Procede a `--execute`
- **Viabilidad ALTA + YELLOW**: Investiga mas, posible oportunidad
- **Viabilidad BAJA + RED**: Cambia de nicho o modo

---

## 8. Ejemplo Practico: Fitness Yoga Mujeres 40+

### Paso 1: Validar Nicho

```bash
python3 -m ai_monetize --validate "fitness yoga mujeres 40+"
```

Resultado: GREEN (88/100)
- Trend Growth: ALTO (93/100) — nicho en crecimiento
- Monetization: MUY ALTA (95/100) — 4+ income streams
- Competition: MEDIA (68/100) — espacio para diferenciarse
- Repeatability: ALTA (88/100) — contenido infinito

### Paso 2: Dry-Run Pipeline

```bash
python3 -m ai_monetize --mode A --niche "fitness yoga mujeres 40+" --dry-run
```

Resultado:
- Modo recomendado: A (Faceless) o B (AI Character)
- Budget: $0-20/mes (Modo B) o $0-100/mes (Modo A)
- Timeline a $5k/mes: 4-6 meses
- Plataforma principal: TikTok + Instagram Reels
- Monetizacion: Passes + productos digitales + affiliate

### Paso 3: Ejecutar (cuando estes listo)

```bash
python3 -m ai_monetize --mode B --niche "fitness yoga mujeres 40+" --execute
```

Esto genera:
- `/opt/ai-monetize/projects/fitness_yoga_mujeres_40+_*/01_PLAN.md`
- Plan completo con stack, budget, timeline
- Templates de prompts para KREA 2
- Estructura de proyecto lista para empezar

### Paso 4: Siguientes Pasos (desde el plan generado)

1. Crear personaje AI (ver character sheets en guides/foundation/)
2. Configurar KREA 2 en Kaggle (ver docs de setup)
3. Entrenar LoRA para consistencia (ver guias de LoRA)
4. Generar batch de 25 fotos iniciales
5. Warm-up cuentas 7-10 dias (growth/01_WARMUP.md)
6. Publicar 15 posts/semana
7. Configurar Passes con tiers de suscripcion

---

## 9. Ejemplo Practico: AI Character Cosplay Anime

### Paso 1: Validar Nicho

```bash
python3 -m ai_monetize --validate "cosplay anime"
```

Resultado: GREEN (85/100)
- Modo B (AI Character): ALTA compatibilidad
- Modo C (Cine Fusion): MEDIA compatibilidad (cosplay + cinematografico)
- Costo mensual: $0-20 (Modo B, Kaggle gratuito)

### Paso 2: Dry-Run

```bash
python3 -m ai_monetize --mode B --niche "cosplay anime" --dry-run
```

Resultado dry-run:
- 32,500 tokens de media ahorrados
- $3.70 evitados en APIs
- 35 min de render ahorrados
- Viabilidad: ALTA

### Paso 3: Ejecutar

```bash
python3 -m ai_monetize --mode B --niche "cosplay anime" --execute
```

### Paso 4: Stack Recomendado

```
KREA 2 Turbo (Kaggle, $0)
  → genera frames base del personaje
  → AnimateDiff para video
  → LoRA entrenado con 20 fotos del personaje
ComfyUI (local o Kaggle)
  → Workflows de generacion batch
  → ControlNet para consistencia de poses
Passes + RM11
  → Plataformas de monetizacion
  → Tiers: $9.99 / $14.99 / $49.99
```

---

## 10. Estructura de Proyecto Generado

Cuando ejecutas `--execute`, se crea un proyecto en `projects/`:

```
projects/<nicho>_<timestamp>/
├── 01_PLAN.md           # Plan completo de ejecucion
├── 02_CONTENT/          # Assets de contenido (vacio, para llenar)
├── 03_GROWTH/           # Estrategias de crecimiento (vacio)
├── 04_MONETIZATION/     # Config de monetizacion (vacio)
├── 05_ASSETS/           # Imagenes/videos generados (vacio)
└── pipeline.json        # Datos del pipeline en JSON
```

### 01_PLAN.md

Contiene:
- Resumen del proyecto (nicho, modo, stack, costos)
- Resultados de cada etapa del pipeline
- Checklist de acciones pendientes

### pipeline.json

Contiene todos los datos estructurados del pipeline para
procesamiento automatico o integracion con otras herramientas.

---

## 11. Monetizacion en Cascada

La monetizacion sigue una progresion de menos a mas esfuerzo:

### Nivel 1: Creator Fund (Pasivo, bajo ingreso)

```
TikTok Creator Fund: $0.02-0.04 / 1,000 views
YouTube AdSense: $4-8 CPM
Instagram Bonus: $0-500/mes (por invitacion)
```

### Nivel 2: Plataformas de Suscripcion (Medio esfuerzo)

```
Passes (recomendado, 10% comision):
  Free: $0 (teasers, gancho)
  Basic: $9.99/mes (fotos exclusivas)
  Premium: $14.99/mes (videos + BTS)
  VIP: $49.99/mes (chats 1:1, custom content)

RM11 (10% comision, nichos especificos)
Fanvue (15% comision, AI creators establecidos)
Patreon (8-12% comision, educativo)
Ko-fi (0% comision + Stripe)
```

### Nivel 3: Productos Digitales (Alto margen)

```
Prompt packs: $14.99 (50 KREA 2 prompts para [nicho])
Presets LUT: $19.99 (color grading usado)
E-book: $14.99 ("How I started an AI influencer")
Video course: $49.99 ("Create an AI influencer from scratch")
LoRA pack: $29.99 (personaje base + prompts)
Done-for-you: $499 (setup completo de personaje + LoRA)
```

### Nivel 4: Brand Deals (Escala)

```
SponsorBridge: $50-500 por post
Billo: $100-1000 por campania
Direct brand outreach: $500-5000 por acuerdo
```

### Revenue Estimates por Mes

```
Mes 1-3:    $0-200   (construyendo audiencia)
Mes 4-6:    $500-2,000 (audiencia + primeros productos)
Mes 7+:     $2,000-5,000 (multiple income streams)
```

---

## 12. Roadmap y Proximos Pasos

### v0.1.0 (Actual) — Nucleo Funcional
- [x] CLI con menu interactivo
- [x] Pipeline 8 etapas con dry-run
- [x] Modos A/B/C/D
- [x] Validador Green Grid (120 nichos)
- [x] Project scaffolding
- [x] Templates base por modo
- [x] Motor de render (ComfyUI + Replicate)
- [x] Multi-cuenta y portfolio
- [x] Web API REST
- [x] Docker + CI/CD

### Proximos
- [ ] Dashboard Web UI
- [ ] Deploy Multi-Servicio
- [ ] Licensed-one integration

---

## 13. Preguntas Frecuentes

### Que es exactamente dry-run?

Ejecuta las 8 etapas pero salta CREATE y RENDER (las unicas que generan
media). Las otras 6 etapas se ejecutan con datos reales. Zero gasto.

### Cual modo es mejor para empezar?

Depende de tu situacion:
- **$0 presupuesto**: Modo B (Kaggle gratuito, solo requieres tiempo)
- **Contenido educativo**: Modo A (MoneyPrinterTurbo, facil)
- **Experiencia en video**: Modo C (calidad premium)
- **Ya tienes un sistema**: Modo D (escala)

### Cuanto tiempo toma de idea a $5k/mes?

| Modo | Timeline | Riesgo |
|------|----------|--------|
| A: Faceless | 4-6 meses | Bajo |
| B: AI Character | 4-6 meses | Medio |
| C: Cine Fusion | 3-5 meses | Medio-Alto |
| D: Auto-Pilot | 2-4 meses | Alto |

### Puedo cambiar de modo despues?

Si. El pipeline es agnostico. Puedes empezar en Modo A y migrar a B o C.
Cada modo tiene su propio stack y estrategia de monetizacion.

### Donde encuentro prompts especificos para mi nicho?

- Modo A: `templates/mode_a_faceless/prompts_base.md`
- Modo B: `templates/mode_b_character/prompts_base.md`
- Modo C: `templates/mode_c_cine/prompts_base.md`
- Modo D: `templates/mode_d_autopilot/prompts_base.md`

### Que hago despues de ejecutar el pipeline?

1. Lee el plan generado en `projects/<proyecto>/01_PLAN.md`
2. Sigue la checklist de acciones
3. Configura las herramientas del stack
4. Genera assets iniciales
5. Inicia warm-up de cuentas (7-10 dias)
6. Publica y mide resultados
7. Ajusta estrategia basada en datos

---

*AI-Monetize v1.0.0 — Sistema unificado de monetizacion con IA*
*"Dry-run primero, ejecuta despues. No gastes tokens en ideas que no usaras."*
