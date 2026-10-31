<!-- ================================================================= -->
<!-- Development By Angel Esquivel (Automation) [AI-MONETIZE 2026] -->
<!-- ================================================================= -->
<!-- AI-MONETIZE - Generador de prompts de monetizacion IA -->

# Pendings — Items evaluados para el futuro

## Deuda de lint restante (auditoria 2026-10-21)

`ruff check ai_monetize/` pasa limpio con el set configurado en `pyproject.toml`
(`E4,E7,E9,F,I,UP,B`). Queda deuda fuera de ese set, que requiere revision semantica
y no fix automatico:

| Regla | Casos | Que es | Revision necesaria |
|-------|:-----:|--------|--------------------|
| BLE001 | 55 | `except Exception` ciego | Distinguir excepciones esperadas por bloque |
| RUF012 | 10 | Defaults mutables en clases | Clasificar con `ClassVar` o `field(default_factory=...)` |
| S110 | 6 | `try/except/pass` | Verificar que el silencio sea intencional |
| ASYNC210 | 4 | HTTP bloqueante en async | Cambiar a cliente async (aiohttp/httpx) |
| PERF102 | 6 | Iteracion de dict ineficiente | Aplicar `.values()` / `.items()` |
| FLY002 | 4 | Concat de strings | Convertir a f-string |
| SIM102/SIM117/PIE810/RUF034/PYI034 | 6 | Simplificaciones menores | Mecanico |

Criterio de activacion: abrir un tramo dedicado de hardening de lint cuando el CI
incorpore el set completo, o cuando se toque el archivo por otra razon.

### Ya resuelto en la auditoria 2026-10-21

- F821/F823 (3 bugs reales): `Path` y `aiohttp` referenciados antes de asignar
  (UnboundLocalError), `output_dir` indefinido en `RenderEngine._exec_render`.
- F401/I001/UP045/UP017/F541/RUF022/UP037 (253 fixes automaticos seguros).
- F841 (8 variables muertas), B007 (6 loops), B904 (5 raise sin `from`), E741 (5 nombres ambiguos).
- Faltantes de export publico: `validate` no estaba en `ai_monetize.__init__` (el CI lo importaba); `__all__` declarado en `ai_monetize`, `analytics` y `publish.providers`.
- 6 `__init__.py` y `docs/VERACIDAD_MMO.md` sin header/footer (Regla 14).

## Versionado

Resuelto parcialmente el 2026-10-21: el proyecto se inicializo como repositorio git
(`git init -b main`) por decision del dueno, con `.gitignore` ya cubriendo `venv/`,
`projects/`, `backups/` (`*.backup.*`), caches y `graphify-out/`.

Pendiente: `graphify hook install` NO se ejecuto porque el binario `graphify` no esta
instalado en el host (solo existe la skill en
`~/.config/opencode/skills/graphify/`, que asume el CLI en el PATH). Instalar graphify
y luego correr `graphify . --code-only && graphify hook install` en la raiz del repo.

## OpenReply — Automatización Instagram Comment → DM

**Repo**: diwenne/openreply (MIT, self-hosted, Next.js/Prisma/PostgreSQL/Redis/BullMQ)
**Stack**: Meta Graph API oficial (no scraping, no password), roles team, multiple IG accounts
**Costo**: hosting propio ($5-15/mes) vs ManyChat ($15-69/mes)

### Gap que resuelve
ai-monetize publica contenido en Instagram (publish/providers/instagram.py) pero NO captura leads automaticamente. El loop se corta en publish.

Nota 2026-09: la letra "D" ahora se usa para Websites PyMEs. Ver `docs/MODES.md`.

OpenReply cerraria el ciclo de captura para contenido (Modo A), con el flujo:

```
PublishStage → OpenReply monitorea comentarios → keyword-triggered DM →
follow-gate → tracking link → CTR por campaña
```

### Por qué no ahora
- Integra un servicio standalone Next.js — no es un módulo Python del pipeline
- Requiere desplegar Postgres + Redis en producción
- La política de Meta limita a 750 DMs/hora (cola manejable, pero requiere monitoreo)
- El valor se activa cuando Mode D tiene cuentas activas publicando → necesita tracción previa
- El debate "agentes que reemplazan empresas enteras" (alassafi.ai, Paul Cheek/MIT) es marketing, no especificaciones técnicas verificables

### Criterio de activación
Re-evaluar cuando:
1. ai-monetize Mode D tenga ≥3 cuentas publicando activamente
2. El costo de ManyChat supere $50/mes en cuentas gestionadas
3. Se requiera tracking de conversión comment→DM→lead por campaña

### Fuentes
- github.com/diwenne/openreply
- tryopenreply.vercel.app

<!-- End Development By Angel Esquivel (Automation) [AI-MONETIZE 2026] -->
