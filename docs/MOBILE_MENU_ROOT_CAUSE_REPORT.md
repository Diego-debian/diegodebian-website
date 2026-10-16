# Causa raíz del menú móvil

**Estado:** `PARTIAL`

El Preview público fue accesible y permitió reproducir el fallo. La corrección estructural se implementó y se verificó localmente, pero todavía no se publicó; por eso el Preview público continúa mostrando la versión anterior.

## Reproducción pública

URL revisada:

```text
https://feature-phase-01-project-bootstrap-diegodebian-website.diego-debian.workers.dev/
```

Se utilizó Chrome instalado localmente mediante `playwright-core`, con emulación de viewport en 320, 360 y 390 píxeles. El Preview respondió con HTTP 200 y la hoja publicada ya contenía el fondo opaco y `z-index: 30` añadidos en la corrección anterior.

La evidencia demostró que esos valores no resolvían el problema: el header publicado tenía `z-index: 1`, mientras `main` también estaba en `z-index: 1`. Al abrir el menú, `document.elementFromPoint()` devolvió el hero o su `h1` en el centro de los cinco enlaces, no los enlaces mismos.

## Causa raíz demostrada

La regla posterior de la capa premium era:

```css
main, .site-header, .site-footer { position: relative; z-index: 1; }
```

Esa regla hacía dos cosas problemáticas:

1. Rebajaba el header al mismo contexto de apilamiento que `main`.
2. Sustituía `position: sticky` por `position: relative`.

El `z-index: 30` del menú no podía escapar del contexto padre del header para superar al hero. Por eso aumentar la opacidad o el `z-index` del menú de forma aislada no era suficiente. El hero tenía una transformación aplicada por la animación de revelado y, en el orden de apilamiento publicado, quedaba por encima del header durante la prueba de hit-testing.

Elementos que interceptaban los clics reales:

- En el primer enlace: `SECTION.hero.container.is-visible`.
- En los cuatro restantes: `H1` del hero.

## Corrección estructural aplicada

Archivo modificado: `src/styles/global.css`.

La regla se separó para que el header conserve su propio nivel de apilamiento:

```css
main, .site-footer { position: relative; z-index: 1; }
.site-header {
  position: sticky;
  top: 0;
  z-index: 20;
  isolation: isolate;
  /* tokens visuales existentes */
}
```

El menú conserva una prioridad local moderada de `z-index: 2`, dentro del contexto aislado del header. No se usó un valor arbitrariamente alto como sustituto de la corrección del contexto padre.

## Comparación de evidencias

Antes, en el Preview público:

- `public-320-open.png`
- `public-360-open.png`
- `public-390-open.png`

Después, en el build local corregido:

- `local-320-after.png`
- `local-360-after.png`
- `local-390-after.png`

Las evidencias están en:

```text
docs/evidence/mobile-menu-root-cause/
```

En la versión pública, `elementFromPoint()` falló para los cinco enlaces en las tres anchuras. En la versión local corregida, los cinco enlaces fueron el elemento superior en las tres anchuras.

## Validación local posterior

Se ejecutó `astro preview` en `127.0.0.1` con Chrome/Playwright.

Resultados:

- Header: `position: sticky`, `z-index: 20`, `isolation: isolate`.
- Menú: `z-index: 2`, `opacity: 1`, `pointer-events: auto`.
- `elementFromPoint()` devolvió el enlace correcto para los cinco enlaces en 320, 360 y 390 px.
- Clics reales con coordenadas del ratón navegaron a `/`, `/servicios/`, `/portafolio/`, `/sobre-mi/` y `/contacto/`.
- No hubo errores de consola ni respuestas HTTP fallidas.
- No hubo overflow horizontal.
- El menú se cerró al activar un enlace; las páginas nuevas cargaron con `aria-expanded="false"`.

Pruebas técnicas:

| Comando | Resultado |
|---|---|
| `npm.cmd run check` | PASS — 0 errores, warnings y hints |
| `npm.cmd run build` | PASS — 5 rutas estáticas |
| `npm.cmd run test` | PASS |
| Pruebas Chrome/Playwright locales | PASS |
| Preview público después de la corrección | NOT_EXECUTED — no se hizo push |

## Archivos modificados

- `src/styles/global.css`
- `docs/MOBILE_MENU_ROOT_CAUSE_REPORT.md`
- `docs/evidence/mobile-menu-root-cause/`

No se modificó `Header.astro`, porque el comportamiento de estado, `Escape` y `aria-expanded` ya era suficiente una vez corregido el contexto de apilamiento.

## Limitación y siguiente decisión

La causa quedó demostrada y la solución pasa localmente, pero el Preview público todavía contiene el commit `58c922d`. Se necesita autorización independiente para publicar esta corrección y repetir el mismo hit-testing en la URL pública. Hasta entonces, el estado correcto es `PARTIAL`, no `PASS`.

STOP — ESPERAR REVISIÓN DEL USUARIO. NO PUBLICAR TODAVÍA.
