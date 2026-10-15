# CLAUDE.md — Calco (demo estudio de tatuajes)
Web demo de un estudio de tatuajes ficticio ("Calco") para enseñar a dueños de estudios: un único `index.html`, sin backend, reserva por WhatsApp/email.

## Stack
- HTML + CSS + JavaScript sin dependencias, todo en `index.html`.
- Google Fonts: Shrikhand (titulares, aire de rótulo pintado) + Archivo (texto).
- Ilustraciones SVG para flash y portada; la galería usa fotos reales de tatuajes de Unsplash (hotlink a images.unsplash.com, licencia libre).
- Despliegue: Cloudflare Workers con assets estáticos (worker `demo-tatuaje-3`, cuenta minisobri26), Git integration con el repo `AleexX26/demo-tatuaje-3`. URL: https://demo-tatuaje-3.e275.workers.dev

## Arquitectura
- `index.html`
  - `<script>` al principio del `<head>`: **datos editables** `ESTUDIO` (con `telefono` y `cifras`), `ARTISTAS`, `TRABAJOS`, `FLASH`, `TEMATICAS`, `PROYECTO` (demo de sesiones), `DIA_SESION`, `ZONAS_DOLOR`, `INVITADOS`, `SERVICIOS`, `RESENAS`.
  - Orden de secciones: portada → cintas de temáticas → trabajos → sesiones → artistas → invitados → flash → servicios (tablón) → clientes (cifras + reseñas) → cómo funciona → mapa del dolor → estudio → cuidados/FAQ → reserva → pie.
  - `<style>`: tokens en `:root` (papel, tinta, violeta de calco, acento rojo) y modo oscuro con `prefers-color-scheme`.
  - `<svg>` oculto con defs compartidas: patrones `#punteado`, `#punteado-denso`, `#rayado`; filtros `#temblor` (línea de calco), `#goma` (sello), `#sangrado` (borde de tinta del entintado).
  - `<script>` final: `MOTIVOS` (dibujos de flash en caja 200×200), `svgPieza()` (capa calco violeta + capa tinta + nombre), `svgSello()`, portada entintable, galería con filtros, vista previa, lightbox, artistas, flash, pasos, plano, FAQ y formulario.
- Flujo de reserva: botones "Quiero algo así" / "Lo quiero" / "Reservar con…" llaman a `rellenarReserva()` → el formulario genera texto → abre `wa.me` o `mailto:`.
- `PRODUCT.md` y `.impeccable/` son contexto de la skill de diseño (no se publican: `.impeccable/` está en `.gitignore`).

## Comandos
- Ver en local: abrir `index.html` con doble clic, o `npx --yes http-server -p 4321 -c-1 .` (config en `.claude/launch.json`, nombre `web`).
- Sin build ni tests. Deploy: push a `main` → Cloudflare redespliega solo (~2-4 min). `wrangler.jsonc` copia `index.html` a `publico/` (ignorado en git) y sirve solo esa carpeta, así README/CLAUDE.md no se publican.
- Probar el despliegue sin subir: `npx --yes wrangler@4 deploy --dry-run`.

## Decisiones
- 2026-10-15: v3 colorida: paleta crema/rosa/amarillo/turquesa/violeta, marcos de colores con sombra dura y galería con fotos realistas de Unsplash. Misma estructura que Tatuajes-2.
- 2026-10-15: ampliada tras revisar todas las secciones de lamanuelatattoo.com (temáticas, zonas con dolor, invitados, servicios/anestesia/academia, reseñas, contacto con teléfono, legales) para que la demo parezca casi terminada. Reseñas y cifras inventadas a petición del usuario, solo con iniciales; el pie avisa de que son ficticias.
- 2026-10-15: efectos de tatuaje: titulares h2 que se calcan y entintan al entrar, salpicadura de tinta al hacer clic, zumbido de máquina al pasar por los botones, barra de progreso roja bajo la cabecera, inclinación 3D en la galería, cintas y reseñas en desfile continuo (paran con hover; quietas con reducir movimiento).
- 2026-10-15: Cloudflare Workers (no Pages heredado), igual que demo-tatuajes y portafolio: `workers_dev: true` y build propio en `wrangler.jsonc` que copia index.html a publico/.
- 2026-10-15: revisión final aplicada: rojo solo en acciones, precios del flash y sellos; esquinas rectas en chips; el título de cada pieza va rotulado en la hoja (el h3 del pie queda solo para lectores de pantalla salvo si hay foto).
- 2026-10-15: dirección de arte "Calco que se entinta": cada dibujo aparece como línea violeta de stencil térmico y se entinta en negro/rojo (scroll, cursor o al aparecer). El violeta solo vive en dibujos sin entintar; el rojo es el único acento (acciones, precios, sello).
- 2026-10-15: datos de contacto como marcadores (`[DIRECCIÓN]`, `[CIUDAD]`, `[BARRIO]`, `[CORREO]`, `[WHATSAPP]`, `[INSTAGRAM]`, `[INSTAGRAM_<ARTISTA>]`) por petición del usuario; nada tomado de las webs de referencia.
- 2026-10-15: sin datos válidos, WhatsApp abre `https://wa.me/?text=…` (elegir contacto) y el email abre `mailto:` sin destinatario, para que la demo funcione igual.
- 2026-10-15: modo oscuro = "pared del estudio": fondo tinta y los dibujos siguen sobre hojas de papel (no se invierten).
- 2026-10-15: un único archivo en vez de Astro/React: no aporta nada a una landing estática y facilita que el cliente lo edite.

## Patrones reutilizables
- **Tatuaje sobre piel por estados**: un mismo SVG con grupos `.boceto`, `.calco`, `.irrita` (halo rojo desenfocado) y `.tinta` (mix-blend multiply, `--d-bg:#fff` para que el papel desaparezca); la clase `ses-<estado>` del svg decide qué se ve (`iniciarSesiones()`).
- **Mapa del dolor**: silueta en `SILUETA` (formas con `lado` para espejo) + datos en `ZONAS_DOLOR`; gotas rojas como medidor (`iniciarDolor()`).
- **Ojo al insertar código con String.replace**: `$` en la cadena de reemplazo se convierte en `# CLAUDE.md — Calco (demo estudio de tatuajes)
Web demo de un estudio de tatuajes ficticio ("Calco") para enseñar a dueños de estudios: un único `index.html`, sin backend, reserva por WhatsApp/email.

## Stack
- HTML + CSS + JavaScript sin dependencias, todo en `index.html`.
- Google Fonts: Shrikhand (titulares, aire de rótulo pintado) + Archivo (texto).
- Ilustraciones SVG para flash y portada; la galería usa fotos reales de tatuajes de Unsplash (hotlink a images.unsplash.com, licencia libre).
- Despliegue: Cloudflare Workers con assets estáticos (worker `demo-tatuaje-3`, cuenta minisobri26), Git integration con el repo `AleexX26/demo-tatuaje-3`. URL: https://demo-tatuaje-3.e275.workers.dev

## Arquitectura
- `index.html`
  - `<script>` al principio del `<head>`: **datos editables** `ESTUDIO` (con `telefono` y `cifras`), `ARTISTAS`, `TRABAJOS`, `FLASH`, `TEMATICAS`, `PROYECTO` (demo de sesiones), `DIA_SESION`, `ZONAS_DOLOR`, `INVITADOS`, `SERVICIOS`, `RESENAS`.
  - Orden de secciones: portada → cintas de temáticas → trabajos → sesiones → artistas → invitados → flash → servicios (tablón) → clientes (cifras + reseñas) → cómo funciona → mapa del dolor → estudio → cuidados/FAQ → reserva → pie.
  - `<style>`: tokens en `:root` (papel, tinta, violeta de calco, acento rojo) y modo oscuro con `prefers-color-scheme`.
  - `<svg>` oculto con defs compartidas: patrones `#punteado`, `#punteado-denso`, `#rayado`; filtros `#temblor` (línea de calco), `#goma` (sello), `#sangrado` (borde de tinta del entintado).
  - `<script>` final: `MOTIVOS` (dibujos de flash en caja 200×200), `svgPieza()` (capa calco violeta + capa tinta + nombre), `svgSello()`, portada entintable, galería con filtros, vista previa, lightbox, artistas, flash, pasos, plano, FAQ y formulario.
- Flujo de reserva: botones "Quiero algo así" / "Lo quiero" / "Reservar con…" llaman a `rellenarReserva()` → el formulario genera texto → abre `wa.me` o `mailto:`.
- `PRODUCT.md` y `.impeccable/` son contexto de la skill de diseño (no se publican: `.impeccable/` está en `.gitignore`).

## Comandos
- Ver en local: abrir `index.html` con doble clic, o `npx --yes http-server -p 4321 -c-1 .` (config en `.claude/launch.json`, nombre `web`).
- Sin build ni tests. Deploy: push a `main` → Cloudflare redespliega solo (~2-4 min). `wrangler.jsonc` copia `index.html` a `publico/` (ignorado en git) y sirve solo esa carpeta, así README/CLAUDE.md no se publican.
- Probar el despliegue sin subir: `npx --yes wrangler@4 deploy --dry-run`.

## Decisiones
- 2026-10-15: v3 colorida: paleta crema/rosa/amarillo/turquesa/violeta, marcos de colores con sombra dura y galería con fotos realistas de Unsplash. Misma estructura que Tatuajes-2.
- 2026-10-15: ampliada tras revisar todas las secciones de lamanuelatattoo.com (temáticas, zonas con dolor, invitados, servicios/anestesia/academia, reseñas, contacto con teléfono, legales) para que la demo parezca casi terminada. Reseñas y cifras inventadas a petición del usuario, solo con iniciales; el pie avisa de que son ficticias.
- 2026-10-15: efectos de tatuaje: titulares h2 que se calcan y entintan al entrar, salpicadura de tinta al hacer clic, zumbido de máquina al pasar por los botones, barra de progreso roja bajo la cabecera, inclinación 3D en la galería, cintas y reseñas en desfile continuo (paran con hover; quietas con reducir movimiento).
- 2026-10-15: Cloudflare Workers (no Pages heredado), igual que demo-tatuajes y portafolio: `workers_dev: true` y build propio en `wrangler.jsonc` que copia index.html a publico/.
- 2026-10-15: revisión final aplicada: rojo solo en acciones, precios del flash y sellos; esquinas rectas en chips; el título de cada pieza va rotulado en la hoja (el h3 del pie queda solo para lectores de pantalla salvo si hay foto).
- 2026-10-15: dirección de arte "Calco que se entinta": cada dibujo aparece como línea violeta de stencil térmico y se entinta en negro/rojo (scroll, cursor o al aparecer). El violeta solo vive en dibujos sin entintar; el rojo es el único acento (acciones, precios, sello).
- 2026-10-15: datos de contacto como marcadores (`[DIRECCIÓN]`, `[CIUDAD]`, `[BARRIO]`, `[CORREO]`, `[WHATSAPP]`, `[INSTAGRAM]`, `[INSTAGRAM_<ARTISTA>]`) por petición del usuario; nada tomado de las webs de referencia.
- 2026-10-15: sin datos válidos, WhatsApp abre `https://wa.me/?text=…` (elegir contacto) y el email abre `mailto:` sin destinatario, para que la demo funcione igual.
- 2026-10-15: modo oscuro = "pared del estudio": fondo tinta y los dibujos siguen sobre hojas de papel (no se invierten).
- 2026-10-15: un único archivo en vez de Astro/React: no aporta nada a una landing estática y facilita que el cliente lo edite.

; usar slice o función de reemplazo.
- **Dibujo entintable**: dos copias del mismo SVG, `.calco` (violeta, filtro `#temblor`, desplazada 1.6px) debajo y `.tinta` encima revelada con `clip-path` (galería) o con `<mask>` de círculos (portada); en `index.html`, `svgPieza()` y `dibujoHero()`.
- **Estilos de tatuaje por clase CSS**: el mismo motivo cambia según `.e-fine`, `.e-black`, `.e-real`, `.e-trad`, `.e-letter` (clases `o/r/k/p/s/tb/tf`).
- **Sello de goma**: filtro `#goma` (feTurbulence + feColorMatrix) sobre SVG o HTML (`.agenda`, `.estampa`, sello del artista).
- **Reserva sin backend**: `mensaje()` + `wa.me/?text=` o `mailto:?subject&body`, con estado "Ficha lista" y botón copiar.
- **Capturas sin interfaz**: script CDP con Edge headless (`--remote-debugging-port`, `Emulation.setEmulatedMedia` para claro/oscuro) útil cuando el panel del navegador está pausado.

## Última actualización
2026-10-15
