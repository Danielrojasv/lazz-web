# lazz-web

Sitio público de [Lazz](https://lazz.cl) — landing + páginas legales (privacidad y términos).

HTML/CSS estático sin frameworks. Deploy automático en GitHub Pages al push a `main`.

## Estructura

```
lazz-web/
├── CNAME                ← lazz.cl
├── index.html           ← landing
├── privacidad/          ← política de privacidad (Phase 2)
├── terminos/            ← términos de uso (Phase 2)
├── css/shared.css       ← design system Lazz
├── assets/
│   ├── brand/           ← isotipo + logos
│   └── photos/          ← imágenes hero + secciones (stock Pexels CC0)
└── docs/specs/          ← Spec-Driven Development
```

## Desarrollo local

Cualquier static server sirve. La opción más rápida:

```bash
cd lazz-web
python3 -m http.server 8080
# abrí http://localhost:8080
```

O con Node:

```bash
npx serve .
```

## Deploy

1. Push a `main`.
2. GitHub Pages auto-deploya (Settings → Pages → Source: `main`).
3. Custom domain `lazz.cl` configurado vía `CNAME` + DNS A records en NIC.cl
   apuntando a:
   - `185.199.108.153`
   - `185.199.109.153`
   - `185.199.110.153`
   - `185.199.111.153`

## Design system

Variables en `css/shared.css`:

- **Paleta**: Verde Sabiduría `#1A531A`, Turquesa Claridad `#3AB09E`,
  Beige Base `#E6DECB`, Cobre Logro `#B87333`, Gris Estructura `#5D6166`.
- **Tipografías**: Comfortaa Bold para wordmark, Fraunces (Georgia fallback)
  para body.
- **Acentos**: badge cobre + border cobre destacan el plan anual recomendado
  — mismo lenguaje visual que el paywall de la app.

## Specs

- `docs/specs/lazz-web-launch.md` — landing + legal pages para el launch v1.

## Imágenes stock

`assets/photos/*` son de Pexels bajo licencia [Pexels License](https://www.pexels.com/license/)
(uso comercial libre, sin atribución requerida). Crédito a:

- `hero.jpg` — RDNE Stock project ([Pexels 7092604](https://www.pexels.com/photo/7092604/))
- `secondary-1.jpg` — Yaroslav Shuraev ([Pexels 6282081](https://www.pexels.com/photo/6282081/))
- `secondary-2.jpg` — Karola G ([Pexels 4959797](https://www.pexels.com/photo/4959797/))
