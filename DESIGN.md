# DiegoDebian — design system

## Identity and purpose

DiegoDebian es un estudio independiente de ingeniería de software. El sitio debe transmitir criterio técnico, trabajo cuidadoso y una relación directa con profesionales y organizaciones que necesitan software, integración o automatización. La personalidad visual es tecnológica y contemporánea, sobria y de alta calidad; no busca parecer una terminal, una plantilla de SaaS ni una página de una agencia genérica.

Nebula Sync es el único proyecto público autorizado en el portafolio. Sus capturas son recursos reales entregados para el proyecto y no deben sustituirse por imágenes generadas o de relleno.

## Visual theme

La composición usa fondos claros, una estructura editorial amplia y acentos azul verdoso. El contraste debe permitir leer el contenido sin depender del color. El encabezado permanece visible durante el desplazamiento y el pie de página forma parte del flujo normal del documento.

## Color roles

| Token | Valor | Uso |
| --- | --- | --- |
| `--color-ink` | `#17232d` | Texto principal |
| `--color-ink-soft` | `#51616d` | Texto secundario |
| `--color-paper` | `#f5f7f8` | Fondo general |
| `--color-surface` | `#ffffff` | Tarjetas y superficies |
| `--color-surface-blue` | `#e7f0f3` | Estados suaves y navegación activa |
| `--color-navy` | `#102f43` | Encabezados y bloques de énfasis |
| `--color-slate` | `#234b60` | Variación de azul para superficies |
| `--color-teal` | `#147d7b` | Acento y marca |
| `--color-teal-dark` | `#0d5e5c` | Acento con mayor contraste |
| `--color-line` | `#d5dfe3` | Bordes y separadores |
| `--color-focus` | `#e6a21a` | Indicador de foco visible |

No usar gradientes estridentes, neón, sombras pesadas ni color como único indicador de estado.

## Typography

El texto usa una pila sans del sistema: `Inter`, `ui-sans-serif`, `system-ui`, `Segoe UI`. Las etiquetas técnicas y pequeños metadatos usan una pila monoespaciada. La escala actual combina texto de lectura cercano a `1rem`, descripción del hero entre `1.05rem` y `1.3rem`, títulos de sección entre `2rem` y `3.5rem`, y un título principal responsive entre `2.65rem` y `4.8rem`. Los títulos usan interlineado compacto, `letter-spacing` negativo y `text-wrap: balance` cuando corresponde.

## Layout and spacing

El contenedor máximo es `70rem`; los márgenes laterales se reducen en móvil sin bajar de `1rem`. El espaciado se basa en tokens de `0.5rem`, `0.75rem`, `1rem`, `1.5rem`, `2rem`, `3rem`, `5rem` y `7rem`. El hero usa una composición de dos columnas en escritorio y una columna en pantallas pequeñas, con altura mínima responsive pero sin alturas fijas para el contenido. En anchos próximos a 1366px las acciones deben seguir dentro de la primera pantalla cuando el contenido previsto lo permita.

Los breakpoints actuales son `48rem` para pasar de navegación compacta a composición amplia y `38rem` para el ajuste móvil final. La galería usa tres columnas en escritorio, dos en tablet y una en móvil.

## Components and states

- El encabezado es sticky, con navegación semántica, estado activo mediante `aria-current` y un botón móvil con `aria-expanded` y `aria-controls`.
- Los botones y enlaces tienen estados visibles de hover y foco; el foco usa un contorno ámbar y no depende de pasar el ratón.
- Las tarjetas mantienen superficies claras, borde fino, radio moderado y una sombra ligera. Las transiciones solo animan `opacity` y `transform`.
- La galería conserva la proporción vertical original de las capturas, declara `width` y `height`, usa `alt` descriptivo y carga de forma diferida cuando está fuera de la primera pantalla.
- La animación de entrada se degrada de forma segura: si no hay JavaScript, `IntersectionObserver` o si el usuario prefiere menos movimiento, el contenido queda visible.

## Responsive behavior

La navegación móvil se abre en el flujo de la página y no debe producir desplazamiento horizontal. Las columnas se apilan progresivamente, las acciones pueden envolver y el footer pasa a una disposición vertical. El contenido debe seguir siendo legible a 360px de ancho y no se debe ocultar información esencial para llenar espacios.

## Accessibility rules

Cada página usa un `main` identificable, un enlace para saltar al contenido y navegación con elementos semánticos. Los enlaces y botones deben conservar foco visible; los destinos internos tienen margen de desplazamiento para no quedar bajo el encabezado sticky. Las imágenes deben tener texto alternativo, y las capturas deben mantener dimensiones explícitas para evitar cambios de layout. Se respeta `prefers-reduced-motion` y no se usan elementos interactivos sin nombre accesible.

## Anti-patterns

No introducir terminales decorativas, métricas inventadas, testimonios ficticios, proyectos adicionales, formularios simulados, parallax, frameworks visuales innecesarios, `transition: all`, footer fijo, contenido que desaparece si falla JavaScript ni imágenes que no procedan del proyecto. Cualquier cambio de identidad, paleta o arquitectura requiere una decisión documentada.

## Reference inputs

Este sistema se redactó a partir de la implementación real del repositorio y de una revisión de estas referencias, consultadas el 15 de octubre de 2026: [Anthropic webapp-testing](https://github.com/anthropics/skills/tree/main/skills/webapp-testing), [Vercel Web Interface Guidelines](https://github.com/vercel-labs/agent-skills/tree/main/skills/web-design-guidelines) y [VoltAgent awesome-design-md](https://github.com/VoltAgent/awesome-design-md). Se usaron como criterios de trabajo; no se copiaron sus documentos ni su identidad visual.
