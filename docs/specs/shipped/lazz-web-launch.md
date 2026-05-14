# LAZZ web — landing + legal pages

**Status:** approved
**Owner:** Daniel
**Created:** 2026-05-13
**Approved:** 2026-05-13
**Related specs:** grader-app `shipped/launch-pricing-plans.md`, `shipped/lazz-rebrand-internal.md`

---

## Why

LAZZ está por entrar a App Store y Apple **exige una privacy policy en URL pública** para aceptar el submit. Hoy `docs/PRIVACY.md` está en el repo de grader-app como borrador, pero un Markdown en un repo privado no es URL pública aceptable.

Paralelamente, antes de empujar marketing necesitamos **una landing en lazz.cl** que:
- Comunique qué hace LAZZ a un profe que llegue desde un link de WhatsApp / Instagram.
- Tenga botones a App Store / Play Store cuando esté publicado.
- Hospede los legal docs (privacidad + términos) en URLs limpias (`lazz.cl/privacidad`, `lazz.cl/terminos`).
- Refleje la línea gráfica del producto — no un template genérico de SaaS.

Sin esto: o no podemos submit-ear a Apple (privacy faltante), o submit-eamos con un link feo que daña confianza al primer click.

---

## Goals

- [ ] **Repo `lazz-web` público** con HTML/CSS estático (sin frameworks) hospedado en GitHub Pages, custom domain `lazz.cl`.
- [ ] **Landing** (`/`) con: hero (qué es LAZZ + tagline), demostración visual (mockup phone o GIF), 3-4 features clave, sección de precios (espejo del paywall: Gratis, Ilimitado mensual/anual), CTA a App Store/Play Store (placeholder hasta el submit real), footer.
- [ ] **Privacidad** (`/privacidad`) con la política completa rellenada con datos de Daniel (RUT, dirección, email DPO).
- [ ] **Términos de uso** (`/terminos`) — borrador mínimo que cumpla con Apple/Google review (puede ser conservador, no full legal en v1).
- [ ] **Contacto** (`/contacto`) con form Formspree (nombre, email, mensaje) — mismo patrón que vetastudios.io/contacto pero con estilos Lazz. Redirige a `/contacto/gracias/` post-submit.
- [ ] Línea gráfica fiel a LAZZ:
  - Paleta: Verde Sabiduría `#1A531A`, Turquesa Claridad `#3AB09E`, Beige Base `#E6DECB`, Cobre Logro `#B87333`, Gris Estructura `#5D6166`.
  - Wordmark "Lazz" en **Comfortaa Bold** (font del producto).
  - Body en serif con **Fraunces** (target) o Georgia fallback.
  - Isotipo `L` cursiva — asset `lazz-isotype.png` (ya existe en grader-app, lo replicamos).
- [ ] Responsive desktop + mobile (90%+ del tráfico de profes va a ser mobile).
- [ ] Lighthouse mobile ≥ 90 en Performance, Accessibility, Best Practices, SEO (estático puro lo logra fácil).

---

## Non-goals / Out of scope

- ✗ **Blog / artículos / contenido editorial** — viene después, primero validamos producto.
- ✗ **i18n** — español Chile only en v1.
- ~~✗ Formulario de contacto con backend~~ — **movido a goals 2026-05-13**: durante implementación Daniel pidió replicar el form de vetastudios (Formspree, sin backend propio) con estilos Lazz. Páginas nuevas `/contacto/` y `/contacto/gracias/`.
- ✗ **Analytics avanzado** (Plausible / GA4) — agregar después en otra spec si Daniel lo decide.
- ✗ **A/B testing de la landing** — overkill para v1.
- ✗ **Página de pricing comparativa larga** — el grid del paywall se reusa visualmente en la landing, no es una página propia.
- ✗ **Página de FAQ** — futuro.
- ✗ **Newsletter signup** — futuro.

---

## User-facing changes

### Estructura del sitio

```
lazz.cl
  /                    → Landing (hero + features + pricing + CTA + footer)
  /privacidad/         → Política de privacidad completa
  /terminos/           → Términos de uso
```

Cada path es una carpeta con `index.html` adentro (patrón vetastudios.io), lo que produce URLs limpias sin extensión.

### Landing — ASCII mockup

```
┌─────────────────────────────────────────────────┐
│  [L isotipo]  Lazz                              │ ← Header
│                                                 │
│              Corrige sin límites                │ ← Hero (verde)
│        El asistente de las maestras             │
│        que hicieron escuela.                    │
│                                                 │
│  [Foto teléfono escaneando una hoja]            │ ← Visual demo
│                                                 │
│  ─────────────────────────────────────────      │
│                                                 │
│  Qué hace Lazz                                  │ ← Section title
│                                                 │
│  📷 Escanea con la cámara                       │ ← Feature 1
│  Saca una foto y Lazz detecta las respuestas    │
│                                                 │
│  ✓ Corrige al instante                          │ ← Feature 2
│  Resultados de los 30 alumnos en segundos       │
│                                                 │
│  📊 Análisis por pregunta                       │ ← Feature 3
│  Sabes qué temas reforzar la próxima semana     │
│                                                 │
│  📱 Reportes para apoderados                    │ ← Feature 4
│  Comparte por WhatsApp en un toque              │
│                                                 │
│  ─────────────────────────────────────────      │
│                                                 │
│  Elige tu plan                                  │ ← Pricing section
│                                                 │
│  [Card Gratis]  [Card Ilimitado mensual]        │
│                 [Card Ilimitado anual ⭐]       │
│                                                 │
│  ─────────────────────────────────────────      │
│                                                 │
│  Para las maestras que                          │ ← Brand/closing
│  hicieron escuela.                              │
│                                                 │
│  [Descargar para iOS]  [Descargar para Android] │ ← CTA
│                                                 │
│  ─────────────────────────────────────────      │
│                                                 │
│  Footer: Privacidad · Términos · Contacto       │
│  © 2026 Daniel Rojas Vera · Puerto Montt, Chile │
└─────────────────────────────────────────────────┘
```

### Privacidad — datos a poblar

Los placeholders del borrador `docs/PRIVACY.md` (grader-app) se reemplazan por:

| Placeholder | Valor |
|---|---|
| `[NOMBRE_COMERCIAL]` | Lazz |
| `[RAZON_SOCIAL]` | Daniel Rojas Vera |
| `[RUT]` | 17.889.199-5 |
| `[DOMICILIO]` | Camino San Antonio Km 5.5, Puerto Montt, Región de Los Lagos, Chile |
| `[EMAIL_DPO]` | contacto@vetastudios.io |
| `[URL_PUBLICA]` | https://lazz.cl/privacidad |
| `[FECHA_PUBLICACION]` | fecha de merge/deploy |
| `[PROVEEDOR_HOSTING]` | Railway (US) |
| `[PAIS_HOSTING]` | Estados Unidos |

### Términos de uso

Estructura mínima v1:

1. Aceptación y partes
2. Descripción del servicio
3. Cuenta del usuario
4. Suscripciones y pagos (referencia a Apple/Google como merchant of record)
5. Uso aceptable
6. Propiedad intelectual
7. Limitación de responsabilidad
8. Modificaciones a los términos
9. Ley aplicable (Chile)
10. Contacto

Conservador, no necesita ser comprehensive en v1 — Apple/Google rara vez observan terms a fondo en submit.

---

## Approach

### Stack

Mismo patrón que `vetastudios-web` (verificado funcionando, custom domain en GitHub Pages):
- HTML5 estático puro, sin framework, sin build step.
- CSS shared (`css/shared.css`) + per-page tweaks inline `<style>` cuando aplique.
- Sin JavaScript a menos que sea estrictamente necesario (ej. menú mobile expandible).
- Hosting: GitHub Pages free tier, repo público.
- Custom domain via `CNAME` file + DNS A records de GitHub en NIC.cl.

### Estructura de archivos

```
lazz-web/
├── CNAME                            ← "lazz.cl"
├── README.md
├── index.html                       ← landing
├── privacidad/
│   └── index.html
├── terminos/
│   └── index.html
├── css/
│   └── shared.css                   ← design system Lazz
├── assets/
│   └── brand/
│       ├── lazz-isotype.png         ← copy desde grader-app
│       └── og-image.jpg             ← imagen para Open Graph (WhatsApp/Twitter preview)
└── docs/
    └── specs/
        ├── _template.md
        ├── README.md
        └── lazz-web-launch.md       ← esta spec
```

### Design system

Variables CSS (`shared.css`):

```css
:root {
  --color-verde: #1A531A;     /* Sabiduría — primario, headings */
  --color-turquesa: #3AB09E;  /* Claridad — focus, secundario */
  --color-beige: #E6DECB;     /* Base — background neutro */
  --color-cobre: #B87333;     /* Logro — accents, links, badge */
  --color-gris: #5D6166;      /* Estructura — body text, rules */
  --color-ivory: #F5F4EE;     /* Cards/surfaces */

  --font-wordmark: 'Comfortaa', system-ui, sans-serif;
  --font-body: 'Fraunces', Georgia, serif;
}
```

Comfortaa y Fraunces se cargan via Google Fonts en `<head>` con `display=swap`.

### Deploy

1. Push a `main` → GitHub Pages auto-deploys
2. CNAME apunta `lazz.cl` al hosting de Pages
3. En NIC.cl configurar:
   - `A` records → `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153` (GitHub Pages IPs canónicos)
   - O `CNAME` apex (si NIC.cl soporta CNAME flattening)
4. En el repo de GitHub: Settings → Pages → Custom domain `lazz.cl` + "Enforce HTTPS" ✓

---

## Decisions log

### 2026-05-13 — Repo separado vs subcarpeta de grader-app
- **Considered:** (A) Carpeta `web/` dentro de grader-app; (B) Repo nuevo `lazz-web`.
- **Chose:** (B).
- **Why:** GitHub Pages requiere repo dedicado para el routing limpio + custom domain. Si todo vive en grader-app, hay que servir desde una rama gh-pages o subdirectorio, no es tan limpio. Además los releases del app y los cambios de la landing tienen ciclos distintos — separar el repo deja el commit log más legible. Mismo patrón que vetastudios-web.
- **Cost:** Un repo más para mantener. Asumible.

### 2026-05-13 — HTML estático sin framework
- **Considered:** (A) Astro / Next.js static export; (B) HTML/CSS plano.
- **Chose:** (B).
- **Why:** Landing es 3 páginas, sin estado, sin dynamic content. Astro/Next es overhead para esto. Vetastudios-web funciona perfecto con HTML plano, mismo approach. Cero build step = cero cosa que se rompa entre el code y el deploy.
- **Cost:** Si en el futuro la landing crece (blog, dashboard, etc.) hay que migrar. Lo evaluamos cuando llegue.

### 2026-05-13 — Persona natural como entidad legal
- Daniel publica como persona natural ("Persona física" en Apple Developer Program — confirmed 2026-05-13). Razón social en privacy = "Daniel Rojas Vera". RUT = 17.889.199-5. Domicilio = Camino San Antonio Km 5.5, Puerto Montt.
- **Why:** validación rápida antes de constituir sociedad. Si el revenue lo amerita en 3-6 meses, evaluamos transferir a SpA con Apple App Transfer flow.
- **Cost:** domicilio personal queda en privacy pública. Asumido conscientemente.

### 2026-05-13 — Custom domain lazz.cl
- Daniel ya reservó `lazz.cl` en NIC.cl. La landing va ahí desde el primer deploy.
- Subdominios (ej. `app.lazz.cl` para Universal Links) quedan diferidos hasta que los necesitemos.

---

## Test plan / Definition of Done

### Manual / E2E

- [ ] Landing carga en `https://lazz.cl/` sin warnings de mixed content ni 404s.
- [ ] `/privacidad` y `/terminos` accesibles con URLs limpias (sin `.html`).
- [ ] Footer linkea las 3 páginas, navegación funciona.
- [ ] Responsive: probar en simulador iOS Safari (iPhone 14 size) + Chrome desktop.
- [ ] Open Graph preview se ve bien al pegar `lazz.cl` en WhatsApp (imagen + título + descripción).
- [ ] Lighthouse mobile ≥ 90 en los 4 ejes.
- [ ] CNAME + DNS verificados con `curl -I https://lazz.cl` → 200 OK.

### Tests automatizados

- [ ] Workflow CI mínimo (`.github/workflows/htmlcheck.yml`): validar HTML5 + linkchecker en cada push.

### Docs

- [ ] README del repo explica el deploy + cómo editar.
- [ ] Spec movida a `shipped/` después del primer deploy en lazz.cl funcional.

---

## Phases

### Phase 1 — Repo + design system + landing (1 día)
- Crear repo `lazz-web` en GitHub público.
- `css/shared.css` con design system Lazz (paleta + tipografías + componentes base).
- `index.html` con hero + 4 features + pricing grid + CTA + footer.
- Imagen Open Graph (1200×630 JPG con isotipo + tagline).
- Deploy en GitHub Pages con custom domain `lazz.cl`.

### Phase 2 — Privacidad + Términos (0.5 día)
- `privacidad/index.html` con borrador rellenado.
- `terminos/index.html` con estructura mínima v1.
- Linkeados desde footer de la landing.

### Phase 3 — Polish + docs (0.5 día)
- README del repo.
- CI workflow básico (HTML validator + link checker).
- Verificar Lighthouse + Open Graph.
- Spec → `shipped/`.

---

## Open questions

- [x] ~~Email DPO~~ — resuelto 2026-05-13: `contacto@vetastudios.io` (mailbox existente de Daniel — vetastudios.io es su dominio personal, no entidad legal separada, así que no hay confusión en la cadena de responsabilidad). Migrar a `privacidad@lazz.cl` cuando configure mail en el dominio propio.
- [x] ~~Imagen del hero~~ — resuelto 2026-05-13: stock de Pexels (CC0/comercial libre) — [Photo 7092604 by RDNE](https://www.pexels.com/photo/7092604/) (celular capturando hojas, persona out-of-focus al fondo). Las otras 2 fotos descargadas (Yaroslav-scan-doc + Karola-diagrams) van en secciones secundarias de la landing como visuals.
- [x] ~~Botones de descarga~~ — resuelto 2026-05-13: "Próximamente" con badge oficial de Apple/Google en gris. Cambiar a link real post-submit a stores.
- [x] ~~Página /contacto~~ — resuelto 2026-05-13: NO en v1. Footer linkea a `mailto:contacto@vetastudios.io`.

---

## Links

- PR: <pendiente>
- Specs relacionadas: `grader-app/docs/specs/shipped/launch-pricing-plans.md` (precios espejo de la landing).
- Referencia: [vetastudios.io](https://vetastudios.io) — mismo patrón de deploy.
- Brand kit: `grader-app/assets/brand/lazz-isotype.png`.
- External docs:
  - [GitHub Pages custom domain](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site)
  - [NIC.cl DNS](https://www.nic.cl/registry/dns.do)
