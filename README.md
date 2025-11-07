# Cotizador PWA — Listo para GitHub Pages

Este proyecto está listo para publicarse en **GitHub Pages**.

## 📦 Contenido
- `index.html` — Tu aplicación PWA
- `sw.js` — Service Worker (cache)
- `manifest.webmanifest` — Manifest PWA
- `icons/icon-192.png`, `icons/icon-512.png` — Iconos
  
## 🚀 Publicar en GitHub Pages
1. Crea un repositorio nuevo en GitHub (o usa uno existente).
2. Sube **todos** los archivos de esta carpeta a la raíz del repo.
3. En GitHub: **Settings → Pages → Build and deployment**  
   - *Source*: `Deploy from a branch`  
   - *Branch*: `main` (o `master`) — */ (root)*
4. Abre la URL que GitHub te dará (por ejemplo: `https://tuusuario.github.io/tu-repo/`).

> Si publicas dentro de una subcarpeta (como `tuusuario.github.io/tu-repo/`), este proyecto ya usa **rutas relativas (`./`)**, así que funcionará bien.

## 🧪 Probar instalación
- Abre el sitio desde Chrome/Edge (desktop o Android).
- Verás el prompt de instalación o en el menú: **Instalar app…**.
- En DevTools → **Lighthouse → PWA** puedes validar que sea instalable.

¡Listo!