# Corrección del menú móvil

**Estado:** `PASS`

Se corrigió exclusivamente la navegación móvil en la rama `feature/phase-01-project-bootstrap`. No se modificó `main`, no se creó commit, no se realizó push y no se cambiaron Cloudflare, DNS, túneles ni GestionCore.

## Causa raíz

El menú se renderizaba como un elemento absoluto dentro del encabezado, pero la capa premium solo heredaba el fondo genérico y no declaraba una prioridad de apilamiento ni una superficie explícitamente opaca para el estado abierto. La interacción tampoco tenía un manejador para cerrar el menú con `Escape`.

## Corrección implementada

### `src/components/Header.astro`

- Se centralizó el cierre en `closeMenu()`.
- Se mantiene sincronizado `aria-expanded` al cerrar por enlace o por `Escape`.
- Al cerrar con `Escape`, el foco vuelve al botón `Menú`.
- Se conservaron los enlaces, la navegación existente y el comportamiento de cierre al seleccionar una opción.

### `src/styles/global.css`

En la regla responsive premium del menú se añadió:

- fondo `rgb(3 12 24 / 98%)`;
- `z-index: 30`;
- `opacity: 1`;
- `pointer-events: auto`;
- sombra discreta;
- altura mínima de `2.75rem` para los enlaces;
- altura mínima de `2.75rem` para el botón móvil.

El botón medido después del ajuste tiene 44 px de alto y el primer enlace 49.39 px.

## Pruebas técnicas

| Prueba | Resultado |
|---|---|
| `npm.cmd run check` | PASS — 0 errores, warnings y hints |
| `npm.cmd run build` | PASS — Astro estático |
| `npm.cmd run test` | PASS |
| Cinco rutas estáticas | PASS |
| Imágenes de Nebula Sync | PASS |
| `git diff --check` | PASS |

## Pruebas reales con Chrome y Playwright

Se utilizó Chrome instalado en:

```text
C:\Program Files\Google\Chrome\Application\chrome.exe
```

La automatización se ejecutó con `playwright-core` contra `astro preview` en `127.0.0.1:4321`. Se revisaron:

- `/`
- `/servicios/`
- `/portafolio/`
- `/sobre-mi/`
- `/contacto/`

Resoluciones:

- `360 × 800`
- `390 × 844`
- `430 × 932`
- `1366 × 768`

Resultados observados:

- menú abierto visible en las tres resoluciones móviles;
- fondo computado: `rgba(3, 12, 24, 0.98)`;
- opacidad: `1`;
- `z-index`: `30`;
- `pointer-events`: `auto`;
- ausencia de overflow horizontal en las cinco rutas móviles;
- apertura y cierre correctos;
- cierre con `Escape` y retorno del foco al botón;
- navegación comprobada hacia las cinco rutas;
- foco visible: contorno sólido de 3 px en cian;
- consola sin errores;
- respuestas HTTP fallidas: ninguna;
- cierre del menú al navegar mediante enlace.

## Evidencias

Se generaron 23 capturas reales en:

```text
docs/evidence/mobile-menu-fix/
```

Incluyen capturas cerradas de las cinco rutas en las cuatro resoluciones y capturas abiertas del inicio en `360 × 800`, `390 × 844` y `430 × 932`. No se utilizaron imágenes generadas ni capturas simuladas.

## Estado Git

```text
 M src/components/Header.astro
 M src/styles/global.css
?? docs/MOBILE_MENU_FIX_REPORT.md
?? docs/evidence/mobile-menu-fix/
```

Los cambios están sin confirmar para revisión del usuario. No se modificó Wrangler ni ningún archivo de configuración de Cloudflare.

## Cierre

`MOBILE_MENU_FIX = PASS`

STOP — WAITING FOR USER REVIEW.
