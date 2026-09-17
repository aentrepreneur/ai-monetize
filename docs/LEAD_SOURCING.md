# Lead Sourcing (Modo D) - GBP + sin web

Objetivo: construir un CSV diario de 20 leads que respondan al pitch ganador:
"Te encontre 3 cosas que te cuestan llamadas. Ya hice un draft.".

## Regla de seleccion

Busca negocios con:
- Google Business Profile activo (resenas reales, fotos, telefono)
- Sin website, website roto, o website NO mobile-friendly
- Ticket por lead alto (roofing/HVAC/plomeria/electricista/dentista/abogado)

Evita al inicio:
- Ticket bajo (pelean $50/mes)
- Negocios con 0 resenas

## Flujo manual (10 min)

1. Abre Google Maps.
2. Busca: "{nicho} {ciudad}".
3. Abre el perfil del negocio y revisa:
   - Website (si existe)
   - Boton de llamar / WhatsApp
   - Resenas (cuantas y que dicen)
4. Guarda en CSV.

## CSV recomendado

Columnas minimas:
- business_name,niche,city

Columnas utiles:
- url,phone,whatsapp,email,services,usp,reviews,service_areas,address
- lang,market,theme,plan_tier,image_url,hours

Listas: usa separador `;` o `|` en services/usp/reviews.

## Comandos

Audit rapido (1 URL):

```bash
python3 -m ai_monetize --audit "example.com" --audit-business "Lopez Plumbing" --audit-lang en
```

Batch por CSV:

```bash
python3 -m ai_monetize --mode D --leads-file leads.csv --execute --leads-audit
```
