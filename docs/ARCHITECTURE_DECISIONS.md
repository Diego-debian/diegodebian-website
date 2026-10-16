# Architecture Decisions

Fecha: 2026-10-14

## ADR-001 — Sitio estático con Astro

**Estado:** propuesta aprobada para la siguiente fase.

Astro permite generar HTML, CSS y JavaScript estáticos, que es suficiente para una página profesional sin usuarios, sesiones ni datos dinámicos. El sitio se construirá con `astro build` y su salida prevista será `dist/`.

No se añadirá `@astrojs/cloudflare` mientras no exista una necesidad de renderizado bajo demanda. La documentación de Astro indica que el adaptador Cloudflare se necesita para rutas on-demand, no para un sitio estático.

## ADR-002 — Cloudflare Workers Static Assets

**Estado:** propuesta aprobada para validación en PHASE_05.

La configuración prevista declarará `assets.directory` apuntando a `./dist`. Wrangler podrá cargar los archivos generados junto con el Worker existente. La configuración exacta, el `compatibility_date`, el nombre del Worker y los comandos de Workers Builds deben verificarse contra el dashboard antes de publicarse.

Este plan usa el flujo de Workers Static Assets, no el flujo clásico de Cloudflare Pages: no se usarán `pages_build_output_dir`, `wrangler pages deploy` ni `wrangler pages dev`. Para una configuración estática sin script Worker, `assets.directory` es suficiente; no se añadirá un binding `ASSETS` si no existe código Worker que lo consuma.

## ADR-003 — Sin backend permanente

**Estado:** aprobado.

La página no necesita base de datos, API propia, autenticación, colas ni servidor encendido. El contacto deberá usar un canal real previamente autorizado; no se implementará un formulario que solo aparente recibir solicitudes.

## ADR-004 — Alcance controlado del portafolio

**Estado:** aprobado.

Nebula Sync es el único proyecto autorizado para la primera versión. No se mostrarán GestionCore, Chatbot SIEE, Iguaque QR, SismiToBot ni AulaViva como trabajos terminados.

## ADR-005 — Identidad visual

**Estado:** pendiente de revisión visual.

Se recomienda la propuesta A: técnico sobrio, con alto contraste, tipografía legible, acentos inspirados en sistemas y detalles monoespaciados usados con moderación. La accesibilidad y la lectura tienen prioridad sobre la decoración.

## ADR-006 — Protección de infraestructura existente

**Estado:** aprobado.

No se modificarán `gestioncore-ga10`, `gestioncore.diegodebian.online`, tareas programadas, DNS, certificados, túneles ni reglas de Cloudflare durante la construcción del sitio.
