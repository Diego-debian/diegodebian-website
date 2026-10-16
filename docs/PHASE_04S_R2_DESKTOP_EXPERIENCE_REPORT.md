# PHASE 04S-R2 — Desktop Experience Report

## Estado

**PHASE_04S_R2_DESKTOP_EXPERIENCE = PARTIAL**

Se mejoró la composición para pantallas grandes, se añadió el correo comercial autorizado, se incorporaron microinteracciones discretas y se dio más espacio a la galería de Nebula Sync. Las pruebas técnicas pasaron. La validación visual en 1366 × 768, 1440 × 900, 1920 × 1080 y 2560 × 1440 queda pendiente porque este entorno no dispone de navegador automatizado.

## Diagnóstico inicial

La implementación partía de una base funcional con header sticky, cinco rutas estáticas y una identidad Técnico sobrio. La auditoría del código identificó estas causas concretas:

- El header tenía una altura mínima aproximada de 5rem, pero el hero no reservaba un espacio ligado al viewport; su altura dependía principalmente del padding y del contenido.
- El hero usaba dos columnas y alineación inferior, sin una `min-height` que equilibrara el espacio útil después del header.
- El título tenía un máximo de 12ch y un tamaño máximo de 5.8rem, lo que podía darle demasiado peso visual en monitores grandes.
- La galería de capturas verticales utilizaba cuatro columnas, reduciendo el ancho de lectura de cada imagen.
- Las tarjetas y secciones tenían estados estáticos, aunque ya existían estados hover básicos para botones.
- La página de contacto todavía no mostraba un canal operativo; el correo autorizado aún no había sido incorporado.
- No existía una aparición progresiva de secciones al entrar en el viewport.

El ancho máximo del contenido se mantuvo en `70rem` y el footer ya utilizaba un flujo natural. No se encontraron alturas fijas que recortaran el contenido.

## Cambios de layout

El hero ahora utiliza:

```css
min-height: clamp(30rem, calc(100svh - 5rem), 42rem);
align-content: center;
```

La fórmula considera la altura aproximada del header sticky y evita fijar una altura única para todos los monitores. También se redujeron el padding máximo del hero, el tamaño máximo del título y su anchura de lectura. En móvil se conserva `min-height: auto` y se mantienen los ajustes responsive anteriores.

La galería de Nebula Sync pasó de cuatro a tres columnas en escritorio, dos en tablet y una en móvil. Así las capturas verticales conservan una anchura suficiente para observar la interfaz sin deformación.

El `body` mantiene `min-height: 100vh` y ahora el elemento `main` ocupa el espacio flexible disponible. El footer continúa en el flujo normal y no utiliza `position: fixed`.

## Transiciones y microinteracciones

Se añadió un mecanismo pequeño basado en `IntersectionObserver` nativo:

- Los elementos con `data-reveal` aparecen al entrar parcialmente en el viewport.
- La transición dura 420 ms y usa únicamente opacidad y una traslación vertical breve.
- Si JavaScript falla, el contenido permanece visible porque la clase que oculta elementos solo se añade cuando el script está disponible.
- Si el usuario prefiere movimiento reducido, no se activa la ocultación progresiva y CSS desactiva las transiciones.
- Las tarjetas incorporan cambios sutiles de borde, sombra y elevación en dispositivos que soportan hover.

No se incorporaron dependencias nuevas, parallax, scroll snapping ni animaciones sobre grandes superficies.

## Header y footer

El header sticky existente se reforzó con una sombra ligera, fondo semitransparente y `backdrop-filter`, conservando `top: 0`, `z-index: 20`, el menú móvil y los estados de foco.

El footer se mantiene al final del documento mediante el layout flex del `body` y `main { flex: 1; }`. No se superpone a tarjetas, botones o texto en la estructura generada.

## Correo comercial

Se incorporó el correo autorizado:

`diegodebian.online@gmail.com`

La página `/contacto/` contiene un enlace real:

`mailto:diegodebian.online@gmail.com`

La página de inicio cambió el CTA textual `Contactar` por `Hablemos de tu proyecto`, que conduce a `/contacto/`. No se añadieron teléfono, WhatsApp ni formularios.

## Nebula Sync

Se conservaron las cuatro imágenes auténticas integradas en la subfase anterior. La galería mantiene sus textos alternativos, dimensiones 720 × 1600, carga diferida y rutas estáticas. Nebula Sync sigue siendo el único proyecto del portafolio.

## Archivos modificados

- `src/components/Hero.astro`: marcado para revelación progresiva.
- `src/components/SectionHeading.astro`: marcado para revelación progresiva.
- `src/components/ServiceCard.astro`: marcado para revelación progresiva.
- `src/components/ProjectCard.astro`: marcado para revelación progresiva.
- `src/components/ProjectGallery.astro`: marcado para revelación progresiva.
- `src/layouts/BaseLayout.astro`: script mínimo de `IntersectionObserver` y soporte para `prefers-reduced-motion`.
- `src/pages/index.astro`: CTA `Hablemos de tu proyecto`.
- `src/pages/contacto/index.astro`: correo comercial y enlace `mailto:`.
- `src/styles/global.css`: hero dinámico, galería de tres columnas, microinteracciones, layout flexible del footer y revelación progresiva.

No se modificaron `main`, `wrangler.jsonc`, Cloudflare, DNS, túneles ni GestionCore.

## Resultados de pruebas

| Prueba | Resultado | Evidencia |
|---|---|---|
| `npm.cmd run check` | PASS | 0 errores, 0 advertencias y 0 hints; 17 archivos |
| `npm.cmd run build` | PASS | Salida `static`; cinco páginas generadas |
| `npm.cmd run test` | PASS | Ejecutó `check` y `build` correctamente |
| `npm.cmd audit` | PASS | 0 vulnerabilidades en todas las severidades |
| Cinco rutas | PASS | `/`, `/servicios/`, `/portafolio/`, `/sobre-mi/`, `/contacto/` |
| Correo | PASS | `mailto:diegodebian.online@gmail.com` presente en `dist/contacto/index.html` |
| Imágenes | PASS | Cuatro imágenes presentes en `dist/images/nebula-sync/` |
| Proyectos públicos | PASS | Solo Nebula Sync |
| SSR | PASS | No se encontraron adaptadores ni marcadores SSR |
| Secretos | PASS | No se encontraron patrones de secretos |
| Navegador real | NOT_EXECUTED | No hay herramienta de automatización disponible |
| Resoluciones de escritorio | NOT_EXECUTED | No se inspeccionaron visualmente 1366, 1440, 1920 ni 2560 px |
| Regresiones móviles | NOT_EXECUTED | Requiere navegador o capturas reales |

El hash SHA-256 de `wrangler.jsonc` permanece:

`B25D7EBF304EC0FC2C8B31CDB9B5481CECAD727A3729FE85B28247037DC6C263`

## Node.js y riesgos

El entorno continúa usando Node.js `v22.13.1` y npm `10.9.2`. `undici@8.12.1` declara `node >=22.19.0`; las pruebas pasan, pero la incompatibilidad de engine sigue documentada. No se actualizó Node globalmente.

Los riesgos abiertos son la falta de inspección visual real, la validación pendiente de la configuración de despliegue de Cloudflare y la necesidad de comprobar el comportamiento del hero en monitores grandes antes de aprobar una preview. No se debe afirmar que el hero cabe en todas las resoluciones solo a partir del CSS.

## Estado Git final

- Rama: `feature/phase-01-project-bootstrap`.
- Cambios existentes: conservados.
- Commit: ninguno.
- Push: ninguno.
- `main`: sin cambios.
- Despliegue: no realizado.
- Cloudflare, DNS, túneles y GestionCore: sin cambios.

PHASE_04S_R2_DESKTOP_EXPERIENCE = PARTIAL.

STOP — WAITING FOR USER REVIEW.
