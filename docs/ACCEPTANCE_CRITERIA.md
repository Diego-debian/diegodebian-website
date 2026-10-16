# Acceptance Criteria

## Compilación y tipos

- `npm run build` termina con código cero.
- La salida contiene `dist/` y las rutas esperadas.
- La validación de TypeScript termina sin errores.
- No se declaran versiones o resultados sin haberlos comprobado.

## Navegación y enlaces

- Todas las páginas principales son accesibles desde la navegación.
- No existen enlaces internos rotos.
- Los enlaces a Google Play y otros servicios externos apuntan a destinos reales.
- Los enlaces externos tienen protección adecuada cuando abren otra pestaña.

## Accesibilidad

- Se puede recorrer la interfaz con teclado.
- El foco tiene un indicador visible.
- Los encabezados siguen una jerarquía coherente.
- Las imágenes tienen texto alternativo cuando aportan información.
- El contraste cumple el objetivo WCAG 2.2 AA.
- Las animaciones respetan `prefers-reduced-motion`.

## Responsive y revisión visual

- La navegación funciona en móvil, tableta y escritorio.
- No hay desbordamiento horizontal involuntario.
- El contenido principal sigue siendo legible con zoom.
- La revisión visual se ejecuta en un navegador real; una compilación correcta no sustituye esa revisión.

## SEO y privacidad

- Cada página tiene título y metadescripción apropiados.
- Existe canonical cuando corresponda.
- Open Graph no contiene imágenes inventadas ni datos falsos.
- `robots.txt` y sitemap reflejan las rutas reales.
- No hay cookies de seguimiento ni analítica no aprobada.
- No se exponen secretos, tokens ni variables privadas.

## Portafolio y contenido

- Nebula Sync es el único proyecto destacado.
- GestionCore, Chatbot SIEE, Iguaque QR, SismiToBot y AulaViva no aparecen como proyectos publicados.
- No existen métricas, testimonios, capturas o logros inventados.
- Las competencias se presentan como capacidades, no como certificaciones empresariales no comprobadas.

## Cloudflare

- La preview se puede abrir sin servicios locales activos.
- Workers Static Assets sirve la salida correcta desde `dist/`.
- La configuración usa `assets.directory` y no `pages_build_output_dir`.
- El flujo no usa `wrangler pages deploy`, `wrangler pages dev` ni Workers Sites.
- Las ramas no productivas usan preview y nunca el comando de despliegue productivo.
- `main` está protegida antes de activar un trigger productivo de Workers Builds.
- La preview no se indexa accidentalmente.
- El dominio principal no se modifica durante la preview.
- `gestioncore.diegodebian.online` continúa intacto.

## Estado de esta fase

Estos criterios están definidos, pero todavía no se marcan como PASS porque el sitio aún no ha sido implementado ni desplegado.
