# DiegoDebian.online — Premium implementation plan

Estado: Etapa A completada  
Fase: `PHASE_04S_R5_PREMIUM_DESIGN_IMPLEMENTATION`  
Fecha: 2026-10-15  
Rama revisada: `feature/phase-01-project-bootstrap`

## Resultado de la etapa A

`PHASE_04S_R5_PREMIUM_DESIGN_PLAN = PASS`

Este documento define la reproducción técnica de la referencia aprobada. No implementa todavía el diseño, no modifica componentes ni cambia la arquitectura Astro estática. La implementación requiere una autorización posterior y separada para la Etapa B.

## Referencia verificada

Archivo: `docs/design/reference/diegodebian-premium-approved.png`  
Dimensiones: `935 × 1683 px`  
SHA-256: `DD476B666B6C7F0A5115115B420FD529159FA9660B152C8C42C3FDEF08DB5016`

La imagen se inspeccionó visualmente. Es una composición vertical que presenta, en un mismo flujo, el encabezado, el hero, servicios, portafolio destacado, presentación profesional, contacto y footer. No se usará como fondo ni como sustituto de HTML.

## Diagnóstico comparativo

### Dirección general

La referencia utiliza un estudio tecnológico premium: fondo nocturno casi negro, líneas de circuito, bordes finos azulados, cian luminoso como acento y tipografía clara de alto contraste. El sitio actual conserva una identidad técnico-sobria, pero está construido sobre fondo claro, superficies blancas y acentos teal. La diferencia afecta la atmósfera, la jerarquía y la sensación de producto terminado, por lo que una simple recoloración no sería suficiente.

### Encabezado

La referencia muestra una marca con símbolo `D` cian, navegación compacta y un CTA de contacto destacado en el extremo derecho. El encabezado actual tiene marca textual con un cuadrado teal, navegación funcional y no presenta un CTA permanente. Se puede reutilizar `Header.astro`, su navegación semántica, `aria-current`, el menú móvil y el comportamiento sticky; deberán sustituirse la presentación de marca, los tokens de color, el botón CTA y los estados visuales.

### Inicio y hero

La referencia divide el hero en dos columnas: propuesta de valor a la izquierda y una composición amplia de circuitos, nodos y módulos tecnológicos a la derecha. El sitio actual usa un hero dividido, pero el panel derecho es un SVG reducido con tres nodos y una leyenda. El texto actual también conserva la redacción provisional de fases anteriores y presenta tres acciones.

Para la reproducción se mantendrá la información autorizada y se adaptará su jerarquía: CTA principal hacia Contacto, enlace secundario hacia Portafolio y un bloque visual SVG más amplio. La altura debe seguir siendo flexible; el titular, la descripción y los CTA deben caber en 1366×768 cuando la altura útil lo permita.

### Servicios

La referencia presenta cinco tarjetas oscuras, compactas, con iconos cian, bordes azulados, textos breves y una flecha de acción. La página actual presenta seis servicios autorizados en una cuadrícula clara de tres columnas, con iconos recién incorporados y textos más extensos. La implementación puede conservar los seis servicios reales en la página `/servicios/`, pero debe llevar el tratamiento visual a tarjetas oscuras y revisar la densidad para que el conjunto se parezca a la referencia sin inventar una sexta categoría en la composición de inicio.

### Portafolio

La referencia muestra una sección destacada con un único proyecto: Nebula Sync, una tarjeta horizontal con logo, descripción, etiquetas tecnológicas y CTA. El sitio actual usa una tarjeta textual y una galería grande con cuatro capturas reales. Para acercarse a la referencia, la tarjeta de Nebula Sync debe convertirse en el elemento principal de la sección; las capturas pueden conservarse como evidencia secundaria más abajo o en la página `/portafolio/`, no como sustituto del proyecto.

### Sobre mí

La referencia utiliza una composición editorial de dos columnas: texto con titular destacado a la izquierda y cuatro capacidades explicadas a la derecha, cada una con icono. El sitio actual ya separa narrativa y puntos de enfoque, pero con una composición clara y paneles de otra identidad. Se reutilizará el contenido autorizado sobre desarrollo de software, Física y Educación; se ajustará la composición, no se añadirán títulos académicos, logros ni experiencia no documentada.

### Contacto y footer

La referencia integra el contacto en una banda oscura con borde cian, una pregunta directa y el correo visible con un icono de sobre. El sitio actual tiene dos paneles claros y conserva el `mailto:` correcto. Se reutilizará `ContactLink.astro` y se dará protagonismo a `diegodebian.online@gmail.com` sin añadir formularios, teléfono ni canales nuevos. El footer de la referencia es más denso, oscuro y con enlaces secundarios; solo se incorporarán enlaces que ya estén autorizados.

## Componentes y archivos previstos

### Reutilizar

- `BaseLayout.astro`: metadatos, skip link, slots, scripts de animación y estructura semántica.
- `Header.astro`: navegación, menú móvil y estado activo.
- `Footer.astro`: estructura del pie y enlaces existentes.
- `ButtonLink.astro`: enlaces CTA y estados de foco.
- `ContactLink.astro`: correo público y enlace `mailto:`.
- `ProjectCard.astro`: datos de Nebula Sync y enlace real.
- `ProjectGallery.astro`: capturas auténticas como material secundario.
- `ServiceCard.astro`: datos y accesibilidad, con nueva presentación visual.

### Sustituir o ampliar en Etapa B

- `Hero.astro`: composición gráfica SVG de mayor escala, indicadores y CTA de referencia.
- `src/styles/global.css`: tokens oscuros, layout premium, tarjetas, bordes, glow y responsive.
- `ProjectCard.astro`: logo real, etiquetas y composición horizontal inspirada en la referencia.
- `src/pages/index.astro`: sección de servicios resumida, portafolio destacado, bloque Sobre mí y contacto integrado.
- `src/pages/sobre-mi/index.astro`: cuatro puntos editoriales con iconografía consistente.
- `src/pages/contacto/index.astro`: banda de contacto con el correo como acción principal.
- `public/`: solo después de localizar y verificar los recursos auténticos que falten.

No se prevé incorporar React, Vue, Tailwind, WebGL, SSR, CMS ni dependencias de UI.

## Paleta propuesta

Los valores siguientes son una propuesta técnica aproximada observada en la referencia, no una extracción colorimétrica certificada. Deben validarse durante la implementación con capturas de navegador.

| Token propuesto | Valor aproximado | Uso |
| --- | --- | --- |
| `--premium-bg` | `#020a12` | Fondo principal |
| `--premium-surface` | `#061521` | Tarjetas y bloques |
| `--premium-surface-raised` | `#0b1e2c` | Tarjeta de proyecto y CTA secundario |
| `--premium-line` | `#153b50` | Bordes y divisores |
| `--premium-cyan` | `#16e6f2` | Marca, enlaces y foco visual |
| `--premium-cyan-soft` | `#55f3f5` | Glow y líneas de circuito |
| `--premium-text` | `#f4f8fb` | Titulares y texto principal |
| `--premium-muted` | `#9bb0bf` | Descripciones y metadatos |
| `--premium-deep-blue` | `#071b2b` | Fondo de módulos gráficos |

El color cian debe funcionar también sin glow. No se usará el resplandor como único indicador de interacción. El naranja visible en las capturas internas de Nebula Sync pertenece a la aplicación y no debe convertirse en el color principal del sitio.

## Sistema tipográfico

La referencia usa titulares sans gruesos, compactos y de gran contraste, con etiquetas pequeñas en mayúsculas y tracking amplio. La implementación deberá conservar CSS estándar y evitar instalar una fuente sin autorización. La primera opción es una pila local con `Inter`, `Segoe UI`, `Arial` y `sans-serif`, usando `font-weight: 700–800`, `letter-spacing` ligeramente negativo en titulares y `text-wrap: balance`. Las etiquetas técnicas usarán la pila monoespaciada existente, en cian y con tamaño reducido.

La escala propuesta es:

- Hero: `clamp(2.8rem, 5.8vw, 5.4rem)` con ancho limitado para controlar los saltos.
- Títulos de sección: `clamp(2rem, 3.8vw, 3.4rem)`.
- Texto de lectura: `1rem–1.15rem`, con interlineado suficiente sobre fondo oscuro.
- Etiquetas y metadatos: `0.68rem–0.78rem`, siempre legibles y no solo decorativos.

## Layout de escritorio

El contenedor máximo seguirá entre `70rem` y `76rem`, con márgenes horizontales amplios. El inicio tendrá un hero de dos columnas con una relación aproximada de 45/55: texto y acciones a la izquierda, red SVG y módulos a la derecha. Las secciones se separarán mediante líneas de borde y cambios muy sutiles de superficie, no mediante grandes bloques vacíos.

La sección de servicios usará cinco tarjetas resumidas en la página de inicio para seguir la referencia y mantendrá los seis servicios autorizados en `/servicios/`. La sección destacada de Nebula Sync será horizontal y el bloque Sobre mí se dividirá en dos columnas. El contacto será una banda de ancho completo dentro del contenedor, seguida de un footer oscuro en flujo normal.

## Layout responsive

- Escritorio, desde `64rem`: hero en dos columnas, tarjetas de servicios en cinco columnas o una adaptación de cuatro/tres si el ancho útil no permite lectura, portafolio horizontal y Sobre mí en dos columnas.
- Tablet, entre `48rem` y `63.99rem`: hero en dos columnas más equilibradas, servicios en dos o tres columnas, portafolio apilado parcialmente y navegación todavía visible si cabe.
- Móvil, por debajo de `48rem`: menú desplegable existente, hero en una columna, decoración SVG reducida o simplificada, CTA principal a ancho cómodo, tarjetas en una columna y contacto apilado.
- Móvil estrecho, por debajo de `38rem`: ocultar solo decoración no esencial, nunca texto o acciones; reducir glow, densidad de circuitos y tamaño de módulos para evitar overflow.

En todos los tamaños se mantendrán `min-width: 320px`, foco visible, `scroll-margin-top`, alturas naturales y el correo funcional. La referencia es una composición larga; no se intentará comprimirla con alturas fijas que recorten contenido.

## Animación ambiental

La alternativa técnicamente equivalente a la imagen estática de circuitos es un SVG decorativo inline o un pseudo-elemento con una retícula CSS. La propuesta es:

1. Dibujar entre 8 y 12 rutas de circuito como `path` SVG con `stroke-dasharray`.
2. Animar lentamente `stroke-dashoffset`, `opacity` y pequeños puntos de conexión.
3. Añadir uno o dos halos mediante gradientes radiales CSS, con opacidad baja.
4. Mantener el SVG `aria-hidden="true"`, sin bloquear clics ni formar parte del contenido.
5. Aplicar `will-change` solo al grupo animado y evitar canvas permanente, WebGL, filtros pesados y múltiples sombras animadas.
6. Desactivar la animación y dejar el fondo estático cuando `prefers-reduced-motion: reduce` esté activo.
7. Garantizar que la página se vea completa sin JavaScript; la animación será una mejora progresiva.

La referencia incluye trazos y glow que no necesitan copiarse píxel a píxel. Un SVG paramétrico con la misma jerarquía, densidad y contraste es la alternativa reproducible y más ligera.

## Logo auténtico de Nebula Sync

El inventario actual no contiene un archivo de logo independiente. La captura `docs/img/WhatsApp Image 2026-09-20 at 8.42.32 PM.jpeg` muestra una página de Google Play donde se identifica Nebula Sync y se ve su icono; las demás imágenes son capturas de pantallas de la aplicación. La imagen aprobada también incluye una representación visual del proyecto, pero no constituye por sí sola un recurso fuente del logo.

Para Etapa B se necesita localizar el logo original en el proyecto de Nebula Sync o recibir un archivo autorizado, preferiblemente PNG/SVG con transparencia. No se debe recortar el icono desde una captura de Google Play ni desde la referencia conceptual sin autorización expresa, porque se perderían proporciones, bordes y procedencia. Hasta disponer de ese recurso, la tarjeta debe conservar una solución tipográfica provisional o reservar el espacio sin presentar una imagen como logo auténtico.

## Riesgos

- La imagen de referencia es una captura conceptual larga; no define estados de teclado, menú móvil, hover, foco ni comportamiento de scroll.
- Los colores aproximados pueden producir contraste insuficiente si se usa demasiado glow o texto cian sobre superficies elevadas; habrá que medirlos durante la Etapa B.
- Un fondo SVG demasiado denso puede aumentar el coste de pintura en móviles. Se debe limitar el número de paths y animar solo transformaciones u opacidad cuando sea posible.
- El logo de Nebula Sync no está disponible como recurso independiente y no debe falsificarse.
- Reducir la página de inicio a cinco tarjetas para seguir la referencia puede crear divergencia con los seis servicios autorizados; la decisión propuesta es resumir cinco en inicio y conservar seis en `/servicios/`.
- Las capturas de Nebula Sync deben seguir siendo recursos secundarios y mantener `width`, `height`, `alt` y lazy loading.
- El header sticky y la decoración no pueden cubrir títulos, foco ni CTA.

## Criterios de aceptación visual para Etapa B

- La estructura general se reconoce como un estudio tecnológico premium oscuro sin usar la imagen como fondo.
- El hero mantiene propuesta de valor, CTA de contacto y enlace al portafolio visibles en 1366×768 cuando la altura útil lo permite.
- La navegación de las cinco páginas sigue funcionando, con header sticky, menú móvil y foco visible.
- La paleta, bordes, tipografía, tarjetas y espaciado siguen los roles definidos aquí y en `DESIGN.md` actualizado.
- El fondo tecnológico usa SVG/CSS ligero, no WebGL ni canvas permanente, y se desactiva con `prefers-reduced-motion`.
- Nebula Sync sigue siendo el único proyecto público; el logo integrado debe ser auténtico y su procedencia debe quedar documentada.
- El correo `diegodebian.online@gmail.com` permanece visible y funcional.
- No hay overflow horizontal ni contenido oculto permanentemente.
- Las cinco rutas siguen generándose como HTML estático y `dist/` continúa siendo la salida.
- La equivalencia visual con la referencia se revisa mediante capturas reales y revisión humana; no se certifica solo por pruebas automáticas.

## Estado Git y recomendación

La rama revisada es `feature/phase-01-project-bootstrap`. Los cambios anteriores permanecen sin confirmar; no se creó una rama nueva porque la Etapa A no autoriza reorganizar ni confirmar el trabajo existente. El único archivo añadido en esta etapa es este plan, además de la imagen de referencia que el usuario ya autorizó copiar previamente.

Recomendación: revisar y aprobar este plan, resolver antes de la Etapa B la procedencia del logo independiente de Nebula Sync y autorizar explícitamente la implementación premium. No iniciar todavía la implementación ni despliegues.
