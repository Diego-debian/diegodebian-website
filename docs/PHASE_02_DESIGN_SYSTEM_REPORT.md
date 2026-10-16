# PHASE 02 Design System Report

Fecha: 2026-10-14

## Estado

`PHASE_02_DESIGN_SYSTEM = PASS`

Se implementó el sistema visual provisional de la propuesta A, técnico sobrio. La fase no incorpora contenido comercial definitivo, formularios funcionales, despliegues ni cambios de infraestructura.

## Rama y estado inicial

- Rama utilizada: `feature/phase-01-project-bootstrap`.
- `main` no fue modificada.
- Los cambios no confirmados de Fase 1 se conservaron.
- El remoto y los documentos de Fase 0 y Fase 1 permanecen en el repositorio.
- No se crearon commits ni se hizo `push`.

## Decisiones de diseño

- Fondo claro neutro con superficies blancas y una superficie azul muy pálida.
- Azul profundo para encabezados y superficies de énfasis.
- Azul pizarra para variación de superficies.
- Verde azulado para acciones y acentos.
- Amarillo ámbar para el foco visible.
- Tipografía del sistema para lectura y una fuente monoespaciada solo para etiquetas técnicas.
- Composición basada en espacios amplios, tarjetas contenidas y jerarquía clara.
- No se usaron fotografías, capturas inventadas, iconotecas, Tailwind ni frameworks de interfaz.

La identidad técnica se expresa mediante estructura, etiquetas breves y contraste; no mediante una terminal decorativa llena de comandos.

## Tokens CSS

Los tokens están centralizados en `src/styles/global.css` e incluyen:

- Colores: `--color-ink`, `--color-ink-soft`, `--color-paper`, `--color-surface`, `--color-navy`, `--color-slate`, `--color-teal`, `--color-teal-dark`, `--color-line` y `--color-focus`.
- Tipografía: `--font-sans` y `--font-mono`.
- Espaciado: `--space-1` a `--space-8`.
- Radios: `--radius-sm` y `--radius-md`.
- Sombra: `--shadow-soft`.
- Ancho máximo: `--content-width`.

## Contraste comprobado

Se calcularon los ratios de los pares principales:

- Texto principal sobre fondo claro: `14.86:1`.
- Texto secundario sobre blanco: `6.40:1`.
- Texto blanco sobre verde azulado: `4.94:1`.
- Verde azulado oscuro sobre superficie azul clara: `6.55:1`.
- Texto claro sobre azul profundo: `11.47:1`.
- Azul profundo sobre superficie azul clara: `12.03:1`.

Estos resultados apoyan el objetivo WCAG 2.2 AA para los colores comprobados, pero no constituyen una certificación completa de accesibilidad.

## Componentes implementados

- `Header.astro`: marca, navegación, estado de página actual y menú móvil.
- `Footer.astro`: pie provisional y aviso de alcance.
- `ButtonLink.astro`: enlaces de acción con variantes.
- `Hero.astro`: encabezado principal y panel de estado de fase.
- `SectionHeading.astro`: encabezados de sección reutilizables.
- `ServiceCard.astro`: tarjetas de capacidades provisionales.
- `ProjectCard.astro`: tarjeta reservada para Nebula Sync.
- `ContactLink.astro`: estructura de canales sin datos personales publicados.
- `BaseLayout.astro`: metadatos provisionales, skip link, slots de header y footer.

## Rutas generadas

El build generó cinco rutas estáticas:

- `/`
- `/servicios/`
- `/portafolio/`
- `/sobre-mi/`
- `/contacto/`

El portafolio solo reserva espacio para Nebula Sync. No se publican otros proyectos, métricas, testimonios o capturas.

## Responsive y accesibilidad implementadas

- Breakpoints para navegación, tarjetas y composición en pantallas pequeñas.
- Navegación móvil con botón `button`, `aria-expanded` y `aria-controls`.
- Enlace para saltar al contenido.
- `aria-current="page"` en la navegación.
- Estados `:focus-visible` visibles.
- HTML semántico con `header`, `nav`, `main`, `section`, `article` y `footer`.
- Regla `prefers-reduced-motion`.
- Áreas táctiles de navegación y botones con altura mínima aproximada de 2.9rem.
- `robots` provisional con `noindex, nofollow`.

## Pruebas ejecutadas

- `npm.cmd run check`: PASS; 0 errores, 0 advertencias y 0 hints.
- `npm.cmd run build`: PASS; modo `static`.
- `npm.cmd run test`: PASS; ejecutó validación y compilación.
- Rutas en `dist/`: PASS; se generaron las cinco páginas esperadas.
- Referencias a SSR o adaptadores: PASS; no se encontraron.
- Escaneo de credenciales en código, configuración y assets: PASS; sin patrones de credenciales.
- Contraste de tokens principales: PASS para los pares calculados.
- Wrangler: no ejecutado, conforme a las restricciones.
- Pruebas visuales reales en navegador: `VISUAL_TESTS_NOT_EXECUTED`; no había navegador automatizado disponible en esta sesión.

Durante la primera validación se detectó un error de tipado en el array de servicios. Se corrigió tipando explícitamente las entradas como tuplas de tres cadenas y las pruebas posteriores pasaron.

## Node.js y dependencias

- Node.js disponible: `v22.13.1`.
- npm disponible: `10.9.2`.
- Astro: `7.3.8`.
- TypeScript: `5.9.3`.
- `@astrojs/check`: `0.9.10`.

Continúa la observación de compatibilidad: `undici@8.12.1` declara Node `>=22.19.0`. El proyecto compila y valida correctamente, pero no se declara compatibilidad total del entorno mientras Node permanezca en `v22.13.1`.

## Archivos creados o modificados

Modificados:

- `src/layouts/BaseLayout.astro`.
- `src/pages/index.astro`.
- `src/styles/global.css`.

Creados:

- `src/components/Header.astro`.
- `src/components/Footer.astro`.
- `src/components/ButtonLink.astro`.
- `src/components/Hero.astro`.
- `src/components/SectionHeading.astro`.
- `src/components/ServiceCard.astro`.
- `src/components/ProjectCard.astro`.
- `src/components/ContactLink.astro`.
- `src/pages/servicios/index.astro`.
- `src/pages/portafolio/index.astro`.
- `src/pages/sobre-mi/index.astro`.
- `src/pages/contacto/index.astro`.
- `docs/PHASE_02_DESIGN_SYSTEM_REPORT.md`.

No se modificó `wrangler.jsonc` ni ningún recurso externo.

## Riesgos y pendientes

- Falta una revisión visual real en navegador en los tamaños 360, 390, 768, 1024 y 1440 px.
- El contraste calculado no reemplaza una auditoría completa con lector de pantalla y navegador.
- El texto provisional debe revisarse antes de convertirse en contenido comercial.
- El canal de contacto, los metadatos SEO definitivos y el contenido de Nebula Sync quedan para fases posteriores.
- Node debe actualizarse antes de declarar compatibilidad total del entorno.

## Punto de detención

No se inicia `PHASE_03_CONTENT_AND_PORTFOLIO` automáticamente. La siguiente fase requiere revisión y autorización explícita.
