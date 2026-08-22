# Pendings — Items evaluados para el futuro

## OpenReply — Automatización Instagram Comment → DM

**Repo**: diwenne/openreply (MIT, self-hosted, Next.js/Prisma/PostgreSQL/Redis/BullMQ)
**Stack**: Meta Graph API oficial (no scraping, no password), roles team, multiple IG accounts
**Costo**: hosting propio ($5-15/mes) vs ManyChat ($15-69/mes)

### Gap que resuelve
ai-monetize publica contenido en Instagram (publish/providers/instagram.py) pero NO captura leads. El loop se corta en publish. OpenReply cierra el ciclo Mode D (Auto-Pilot):

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
