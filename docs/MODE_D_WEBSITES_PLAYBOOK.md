<!-- ================================================================= -->
<!-- Development By Angel Esquivel (Automation) [AI-MONETIZE 2026] -->
<!-- ================================================================= -->
<!-- AI-MONETIZE - Generador de prompts de monetizacion IA -->

# Modo D - Websites PyMEs (retainer) - Playbook

Objetivo: convertir tu capacidad tecnica en MRR. No vendes "sitios". Vendes llamadas/leads y tranquilidad.

## La idea del multiplicador

- 1 sitio = un proyecto.
- 1 retainer = un multiplicador (ingreso mensual que compone).
- La web es el gancho. El negocio real es el mantenimiento + upsells.

Matematica simple (default $300/mes):

- 3 clientes x $300/mes = $900 MRR
- 10 clientes x $300/mes = $3,000 MRR
- 20 clientes x $300/mes = $6,000 MRR

## Donde esta la ventaja (2026)

- La mayoria manda spam "te hago una web".
- Lo que funciona: especificidad + valor previo.
- "Ya te hice un draft" abre conversaciones.

Fuentes (observado en web + Reddit):

- Pre-build: mostrar algo antes de pedir dinero sube respuesta.
- Canal principal: Google Maps (negocios con resenas y sin web o con web mala).
- Oferta ganadora: mensualidad + limites claros + cliente sin miedo (cancel anytime).

## Nichos recomendados

Prioriza nichos donde 1 lead vale mucho (alto ticket):

- Roofers, HVAC, plomeria, electricistas
- Dentistas, clinicas
- Abogados

Evita al inicio:

- Negocios con ticket bajo y poco margen (donde te pelean $50/mes)

## Lead sourcing (sin automatizar primero)

1. Abre Google Maps.
2. Busca "{nicho} {ciudad}".
3. Selecciona negocios con:
   - Sin website (o link roto)
   - Website lento/no movil
   - Buenas resenas (prueba social)
4. Guarda 20 leads/dia en CSV.

Tip: usa `docs/LEAD_SOURCING.md`.

## El workflow operativo (diario)

### Paso 1: Generar un draft para 1 lead (5-10 min)

Prepara un JSON brief (minimo):

```json
{
  "business_name": "Plomeria Lopez",
  "niche": "plomeria",
  "city": "San Salvador",
  "phone": "+50370000000",
  "email": "hola@example.com",
  "services": ["Destape", "Fugas", "Instalaciones"],
  "service_areas": ["San Salvador", "Soyapango"],
  "usp": ["Llegamos en 60 min", "Garantia"],
  "reviews": ["Llegaron rapido y resolvieron"],
  "whatsapp": "+50370000000",
  "brand_color": "#14b8a6",
  "lang": "es",
  "market": "latam",
  "theme": "dark",
  "plan_tier": "base",
  "hours": "Lun-Sab 8:00-18:00",
  "website_url": "" 
}
```

Genera el bundle (ver ejemplos en `examples/`):

```bash
python3 -m ai_monetize --mode D --niche plomeria --execute --client-file /ruta/brief.json
```

El output queda en `/opt/ai-monetize/projects/.../06_WEBSITE/`:

- `website/index.html` + `styles.css`
- `OUTREACH.md`
- `OFFER.md`
- `DEPLOY.md`
- `TRACKING.md`

### Paso 2: Outreach (manual, especifico)

Regla: 1 lead = 1 mensaje. Nada de blast.

Usa `OUTREACH.md` como guion.

Pitch ganador (audit basado en evidencia):

```bash
python3 -m ai_monetize --audit "example.com" --audit-business "Lopez Plumbing" --audit-lang en
```

Opcional: si llenas `website_url` en el brief, el bundle genera `AUDIT_OUTREACH.md` automaticamente.

### Paso 3: Cierre

Oferta recomendada (modelo 2026):

- Sin fee de build.
- Sin contrato, cancela cuando quieras.
- El sitio (archivos + contenido) es del cliente.

Planes (default LATAM):
- Base: $300/mes (1 pagina + hosting + 1 cambio/mes)
- Growth: $500/mes (multi-pagina + tracking + copy mensual)

US (premium): ver `OFFER.md` en modo `market=us`.

### Paso 4: Delivery

Deploy manual a Cloudflare Pages (ver `DEPLOY.md`).

Checklist antes de entregar:

- CTA funciona en movil
- Copy directo (sin humo)
- Prueba (resenas/garantias)

### Paso 5: Retencion

No necesitas analytics complejos. Mide dinero influido (ver `TRACKING.md`):

- llamadas
- whatsapp
- formularios
- cierres

Cadencia de follow-up (copy/paste, 2 follow-ups max):

- Dia 0: enviar mensaje + pedir 3 servicios + zona.
- Dia 3: follow-up corto: "Lo pudiste ver? Si me dices 3 servicios + zona lo ajusto y te mando link."
- Dia 7: ultimo follow-up: "Cierro esto hoy. Si quieres que lo deje listo, solo necesito 3 servicios + zona.".

Tracker (para no perder follow-up):

```bash
python3 -m ai_monetize --tracker-add "Plomeria Lopez|draft_sent|+50370000000"
python3 -m ai_monetize --tracker-touch "Plomeria Lopez|follow-up dia 3 enviado"
python3 -m ai_monetize --tracker-summary
```

Gates (para decidir con datos):

- 90 dias: si no llegas a $750 MRR facturado, el problema es distribucion (no producto). Ajusta lead engine.
- $1.5k MRR (5 clientes): punto para reevaluar empleo vs emprendimiento.

Upsell recomendado: AI Receptionist (documentado en `PROMPT.md`) como add-on de llamadas perdidas.

### Meta Ads -> WhatsApp (upsell, una vez que tienen el sitio)

Validado por operadores 2026 (el "trafico directo a WhatsApp", sin landing):

- Anuncio simple con la oferta del cliente + boton a su WhatsApp (usa el numero del brief).
- El gasto lo paga el cliente; tu cobras la gestion: $150-$400/mes (ver OFFER.md, add-ons).
- Ventaja vs landing: sin funnel adicional, conversa y cierras en el chat.
- Riesgos: cuenta publicitaria propia (ban por falta de historial), depende de que el cliente responda rapido en WhatsApp; recuerdalo en cada mensual.

### Adquisicion de tus propios clientes (el anuncio "caballo en venta")

El mismo sistema apunta a ti: para captar PyMEs clientes con presupuesto minimo:

1. Crea una campania Meta: objetivo "trafico/clics a WhatsApp" (sin landing).
2. Copy tipo: "Hago web + leads para tu negocio. Te hago un draft gratis en horas." (sin promesas).
3. Presupuesto de prueba: $5-$10/dia x 5-7 dias (recordar $25-$70 de inversion en adquisicion).
4. Cada lead entra a tu tracker: `--tracker-add "Negocio|lead|contacto"`.
5. El pitch ganador sigue siendo audit + draft ya construidos (seccion Paso 2).

Regla: si en 7 dias no hay conversaciones con duenos, cambia copy o nicho; el problema es mensaje/audiencia, no producto.

## Errores comunes

- Vender "paginas" en vez de "llamadas/leads".
- Ofrecer revisiones ilimitadas.
- Automatizar outreach antes de tener mensaje que funciona.
- Hacer 10 nichos a la vez. Empieza con 1.
- Comprar mentorias por el prompt "garantizamos clientes"; la captacion es TU tarea, nadie te la regala sin coste.

## Fuentes y criterio de veracidad

- Modelo "sistema > sitio" y oferta 2026: analisis propio de industria (Vendasta, casos de agencias, 2026-08/09).
- Tactica "caballo en venta" / trafico directo a WhatsApp: video de Tony Galvez (Clicana, youtube.com/live/AbDgW-tfjA4, 2026-10). Analisis: metodo 80% estandar y alineado con nuestra oferta UPSELL de Meta Ads->WhatsApp; garantias de "5-7 clientes asignados" = marketing sin contrato visible. Adoptamos la tactica; NO el programa pagado.

<!-- End Development By Angel Esquivel (Automation) [AI-MONETIZE 2026] -->
