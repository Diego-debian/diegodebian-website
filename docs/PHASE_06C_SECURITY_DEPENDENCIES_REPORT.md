# PHASE 06C — Security Dependencies

**Estado:** `PARTIAL`

Se revisaron las dependencias después de instalar Wrangler. La actualización compatible de Wrangler corrigió el árbol vulnerable conocido, pero el endpoint de advisories de npm no respondió durante la auditoría final; por eso el resultado completo de `npm audit` queda como `NOT_VERIFIED` y requiere una repetición en un entorno con acceso normal al registro.

## Alcance y estado inicial

- Rama: `feature/phase-01-project-bootstrap`.
- HEAD conservado: `02c47850e28a9564f05c631b4e4404764c056c4f`.
- Node.js: `v24.22.0`.
- npm: `11.19.1`.
- Wrangler inicial: `4.135.0`.
- Configuración conservada: `assets.directory = ./dist` y `previews: {}`.
- No se modificaron páginas, diseño, Cloudflare, DNS, túneles ni GestionCore.

El informe de la fase anterior registraba cuatro vulnerabilidades altas en dependencias de desarrollo. El árbol afectado era `wrangler@4.135.0` → `miniflare@5.20260918.0-alpha` → `sharp@0.35.4` y `undici@7.29.0`. La auditoría inicial no afectaba a las dependencias de producción.

## Diagnóstico y remediación

| Hallazgo inicial | Cadena | Corrección | Resultado |
|---|---|---|---|
| `sharp <0.35.5`, severidad alta; `CVE-2026-96889`, `GHSA-wq5f-xc86-pv6w` | Wrangler → Miniflare → sharp | Wrangler `4.149.0`, que incorpora `sharp@0.35.5` | Corregido en el árbol instalado |
| Avisos altos de `undici` en la rama 7.x | Wrangler → Miniflare → undici | Wrangler `4.149.0`, que incorpora `undici@7.29.1` | Corregido en el árbol instalado |
| Identificadores individuales de los otros avisos de `undici` | El JSON inicial completo no quedó preservado y el endpoint actual no responde | Se evitó inventar identificadores; se actualizaron las dependencias transitivas mediante Wrangler | Requiere repetir `npm audit` con conectividad al registro |
| Conteo total inicial: 4 altas | Dependencias de desarrollo | Actualización menor dentro de Wrangler 4.x | El instalador reportó `found 0 vulnerabilities`; la auditoría JSON posterior quedó bloqueada por el endpoint |

La vulnerabilidad de `sharp` está documentada por el proyecto en [GHSA-wq5f-xc86-pv6w](https://github.com/lovell/sharp/security/advisories/GHSA-wq5f-xc86-pv6w). La actualización de Wrangler que cambia Miniflare a `sharp@0.35.5` aparece en las [versiones del Workers SDK](https://github.com/cloudflare/workers-sdk/releases). El árbol final usa `undici@7.29.1`, por encima de `7.29.0`; la corrección debe confirmarse de nuevo con el servicio de auditoría de npm.

## Cambios aplicados

- `package.json`: Wrangler actualizado de `^4.135.0` a `^4.149.0`.
- `package-lock.json`: árbol reproducible actualizado.
- `wrangler.jsonc`: sin cambios; conserva `name`, `assets.directory` y `previews: {}`.
- No se añadieron overrides, excepciones de auditoría ni dependencias de producción.

La actualización no cambia de versión mayor y mantiene el requisito de Wrangler `>=4.135.0` para Worker Previews.

## Resultados de comandos

| Comando | Resultado |
|---|---|
| `npm.cmd audit --json` inicial | `BLOCKED`: el endpoint bulk de npm devolvió error de conexión/permisos |
| `npm.cmd audit --omit=dev --json` inicial | PASS — 0 vulnerabilidades de producción |
| `npm.cmd audit fix --dry-run --json` | `BLOCKED`: el mismo endpoint no respondió; no se aplicaron cambios automáticos |
| `npm.cmd install --save-dev wrangler@4.149.0` | PASS — instalación terminó con `found 0 vulnerabilities` |
| `npm.cmd exec -- wrangler --version` | PASS — `4.149.0` |
| `npm.cmd ls wrangler miniflare sharp undici --all` | PASS — Miniflare usa `sharp@0.35.5` y `undici@7.29.1` |
| `npm.cmd run check` | PASS — 0 errores, warnings y hints |
| `npm.cmd run build` | PASS — salida estática |
| `npm.cmd run test` | PASS |
| `npm.cmd audit` final | `NOT_VERIFIED`: endpoint bulk de npm no respondió |
| `npm.cmd audit --omit=dev` final | PASS — 0 vulnerabilidades |

Wrangler volvió a mostrar un `EPERM` al intentar escribir logs en `C:\Users\sysadmin\AppData\Roaming\xdg.config`; la versión se imprimió correctamente y no se ejecutaron comandos de preview o deploy.

## Verificación del resultado estático

El build generó correctamente:

- `dist/index.html`;
- `dist/servicios/index.html`;
- `dist/portafolio/index.html`;
- `dist/sobre-mi/index.html`;
- `dist/contacto/index.html`.

También se conservaron los recursos públicos existentes, incluyendo favicon, `robots.txt`, `sitemap.xml` y el logo de Nebula Sync.

## Riesgos pendientes

1. Repetir `npm.cmd audit --json` y `npm.cmd audit fix --dry-run --json` en un entorno donde npm pueda consultar `https://registry.npmjs.org/-/npm/v1/security/advisories/bulk`.
2. Confirmar que el registro reporte cero vulnerabilidades en todo el árbol de desarrollo, no solo en producción.
3. Resolver el `EPERM` de logs de Wrangler si se necesita usar la CLI en este entorno; no afecta al árbol de dependencias.
4. Mantener la revisión humana antes de autorizar el primer preview push.

## Estado Git

Cambios pendientes previstos:

```text
M  package-lock.json
M  package.json
?? docs/PHASE_06A_CLOUDFLARE_PREVIEW_READINESS_REPORT.md
?? docs/PHASE_06B_WRANGLER_SETUP_REPORT.md
?? docs/PHASE_06C_SECURITY_DEPENDENCIES_REPORT.md
```

No se crearon commits ni push. No se incluyeron checkpoints, `node_modules`, `dist`, secretos ni credenciales.

## Cierre

`PHASE_06C_SECURITY_DEPENDENCIES = PARTIAL`

La remediación compatible se aplicó y el árbol instalado ya usa versiones corregidas de `sharp` y `undici`. La fase no puede declararse `PASS` hasta repetir la auditoría completa cuando el endpoint de npm esté disponible.

STOP — WAITING FOR USER REVIEW.
