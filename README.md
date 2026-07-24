# Núcleo Linux UAGRM

Comunidad de Software Libre y Código Abierto de la Universidad Autónoma Gabriel René Moreno.

## Dev

```bash
pnpm install
pnpm dev          # servidor de desarrollo en http://localhost:4321
pnpm build        # genera el sitio estático en dist/
pnpm preview      # previsualiza el build localmente
```

## Estructura del proyecto

```
src/
├── components/
│   ├── Header.astro      # Barra de navegación
│   ├── Footer.astro      # Footer con redes y enlaces
│   └── Slider.astro      # Galería de imágenes auto-sliding
├── layouts/
│   └── Layout.astro      # Layout global (head, body, scripts)
└── pages/
    ├── index.astro       # Página principal
    └── comunidad.astro   # Proyectos y miembros
public/
├── content/
│   ├── projects.json     # Lista de proyectos (editable)
│   └── members.json      # Lista de miembros (editable)
└── *.jpg / *.png         # Imágenes del slider y logo
```

## Modificar contenido

No necesitás recompilar para cambiar proyectos o miembros. Solo editá los JSON.

### Proyectos

`public/content/projects.json`:

```json
{
  "name": "FICCT-OS",
  "description": "Descripción breve.",
  "status": "Desarrollo",
  "links": [
    { "url": "https://github.com/...", "label": "GitHub" },
    { "url": "https://wiki.nluagrm.org/...", "label": "Wiki" }
  ]
}
```

Los iconos se asignan según la URL: `github.com` → GitHub, `wiki.nluagrm.org` → libro, otros → enlace externo.

### Miembros

`public/content/members.json`:

```json
{
  "name": "Nombre",
  "role": "Rol",
  "bio": "Descripción breve.",
  "github": "https://github.com/usuario"
}
```

### Slider (galería de imágenes)

Las imágenes del slider se cargan desde `public/content/slides.json`:

```json
{
  "img": "/imagen.jpg",
  "title": "Título visible al hacer hover",
  "url": "https://ejemplo.com"
}
```

- `img`: ruta a la imagen (debe estar en `public/`)
- `title`: texto que aparece al hacer hover
- `url`: (opcional) link al que redirige al hacer click

Las imágenes deben estar en `public/`.

### Footer (subdominios y redes)

Editá `src/components/Footer.astro`.

### Estilos globales (colores, tema)

Editá las variables CSS en `src/layouts/Layout.astro`:

```css
:root {
  --accent: #4ade80;      /* color principal */
  --accent-hover: #22c55e;
  --bg: #0f0e13;          /* fondo oscuro */
  --text: #f5f5f7;
}
```

## Poner en producción

### 1. Build

```bash
pnpm build
```

Esto genera todo en la carpeta `dist/`. El contenido es 100% estático — HTML, CSS, JS, imágenes y JSON.

### 2. Subir a GitHub (GitHub Pages)

```bash
# Crear repo en GitHub si no existe
git remote add origin https://github.com/tu-usuario/tu-repo.git

# Subir el código fuente
git add .
git commit -m "actualización"
git push origin main
```

Luego en GitHub:
1. Ir a Settings → Pages
2. En "Source" seleccionar **GitHub Actions**
3. Crear `.github/workflows/deploy.yml`:

```yaml
name: Deploy
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
      - run: npm install -g pnpm
      - run: pnpm install
      - run: pnpm build
      - uses: peaceiris/actions-gh-pages@v4
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

Si usás **Netlify** o **Vercel**:
- Conectá tu repo de GitHub
- Comando de build: `pnpm build`
- Directorio de publicación: `dist`

### 3. Usar el CNAME

El archivo `CNAME` ya contiene `nluagrm.org`. Si usás GitHub Pages, configurá el dominio personalizado en Settings → Pages → Custom domain.