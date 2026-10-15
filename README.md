# Calco · demo de web para estudio de tatuajes

Web de una sola página para un estudio de tatuajes: galería filtrable por estilo y artista, fichas de artistas, hoja de flash con precio cerrado, cómo funciona, cuidados y preguntas frecuentes, y una reserva que prepara el mensaje de WhatsApp o email. Todo en `index.html`, sin backend y sin dependencias.

## Cómo personalizarla

Abre `index.html` y edita el bloque **DATOS DE LA WEB** del principio:

| Qué | Dónde |
| --- | --- |
| Nombre, ciudad, dirección, horario, email, WhatsApp, Instagram, color de acento | `const ESTUDIO` |
| Artistas (nombre, especialidad, frase, estilos, Instagram, foto) | `const ARTISTAS` |
| Trabajos de la galería (título, estilo, artista, zona, imagen) | `const TRABAJOS` |
| Flash (nombre, precio, tamaño, disponible, imagen) | `const FLASH` |
| Temáticas de las cintas animadas | `const TEMATICAS` |
| Demo de sesiones de un proyecto (pasos, fechas, material) | `const PROYECTO` |
| Un día de sesión hora a hora | `const DIA_SESION` |
| Mapa del dolor (zona, dolor 1–5, curación, consejo) | `const ZONAS_DOLOR` |
| Artistas invitados (fechas, huecos) | `const INVITADOS` |
| Servicios y precios | `const SERVICIOS` |
| Reseñas (solo iniciales) y cifras del estudio | `const RESENAS` y `ESTUDIO.cifras` |

- Sustituye los marcadores `[DIRECCIÓN]`, `[CIUDAD]`, `[BARRIO]`, `[CORREO]`, `[WHATSAPP]`, `[TELÉFONO]`, `[INSTAGRAM]` y los Instagram de cada artista. Los enlaces legales del pie (privacidad, aviso legal, cookies) apuntan a `#`: enlázalos a las páginas reales.
- Las reseñas, cifras, artistas invitados y el proyecto de sesiones son de muestra: cámbialos por los reales antes de publicar la web de un estudio.
- WhatsApp va con prefijo de país y sin espacios: `34600111222`.
- Para añadir algo, copia un bloque `{ ... }` y cámbialo. Los filtros y los desplegables de la reserva se generan solos.
- Si un trabajo o flash no tiene `imagen`, la web dibuja una ilustración SVG con su nombre. Para usar fotos, crea una carpeta `img/` y pon la ruta: `imagen: "img/golondrina.jpg"`.
- El título y la descripción para Google están en las primeras líneas del `<head>`.

## Publicar

- **Cloudflare** (actual: https://demo-tatuaje-3.e275.workers.dev): Workers & Pages → Create → Continue with GitHub → este repositorio → Implementar. `wrangler.jsonc` ya trae la configuración y cada push a `main` vuelve a publicar.
- **Netlify / Vercel**: importar el repositorio sin comando de build.
- **GitHub Pages**: Settings → Pages → Deploy from branch → `main` / root.

Para verla en local basta con abrir `index.html` en el navegador.
