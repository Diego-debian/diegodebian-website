# PHASE 06A — Cloudflare Preview Readiness

**Estado:** `PARTIAL`

La preparación local está lista, pero la configuración real del Worker `diegodebian-website` no pudo verificarse porque esta sesión no tiene acceso autorizado de solo lectura al panel o a la API de Cloudflare. No se ejecutó Wrangler, no se hizo push y no se modificaron configuraciones externas.

## Resumen de estados

- `VERIFIED`: rama, commit, estado Git, configuración local, build estático, cinco rutas, recursos públicos y ausencia de referencias a localhost.
- `NOT_VERIFIED`: repositorio conectado en Cloudflare, rama productiva, triggers, preview builds, comandos configurados, directorio raíz, modelo de previews, dominios y rutas personalizadas.
- `BLOCKED`: no hay bloqueo local para preparar el preview; la inspección real de Cloudflare está bloqueada por falta de acceso de lectura.
- `REQUIRES_USER_ACTION`: revisar las pantallas de Cloudflare indicadas abajo y autorizar una fase posterior de preview.

## Etapa A — Auditoría local

### Git

| Comprobación | Estado | Resultado |
|---|---|---|
| Rama activa | `VERIFIED` | `feature/phase-01-project-bootstrap` |
| HEAD | `VERIFIED` | `02c47850e28a9564f05c631b4e4404764c056c4f` |
| Mensaje | `VERIFIED` | `feat(site): complete premium DiegoDebian public website` |
| Estado Git | `VERIFIED` | limpio |
| Remoto | `VERIFIED` | `origin` apunta a `https://github.com/Diego-debian/diegodebian-website.git` |

### Configuración Astro y Workers Static Assets

`astro.config.mjs` contiene `output: 'static'` y no configura SSR, adaptador de Cloudflare ni backend.

`wrangler.jsonc` contiene:

```jsonc
{
  "$schema": "./node_modules/wrangler/config-schema.json",
  "name": "diegodebian-website",
  "compatibility_date": "2026-10-14",
  "assets": {
    "directory": "./dist"
  }
}
```

Resultado: `VERIFIED`. El directorio de assets es `./dist`, no hay `main`, bindings, variables privadas ni rutas DNS en el archivo local. El SHA-256 de `wrangler.jsonc` es `B25D7EBF304EC0FC2C8B31CDB9B5481CECAD727A3729FE85B28247037DC6C263`.

La documentación oficial de Cloudflare confirma que `assets.directory` puede apuntar a `./dist/` y que `run_worker_first` es opcional, con valor predeterminado `false`. Esto coincide con la configuración local y con un sitio sin código Worker adicional: [Static Assets — Configuration and Bindings](https://developers.cloudflare.com/workers/static-assets/binding/).

### Scripts locales

`package.json` contiene:

- `npm run build` → `astro build`;
- `npm run check` → `astro check`;
- `npm run test` → `check` y `build`;
- `npm run preview` → `astro preview`;
- `npm run dev` → `astro dev`.

No existe script local `deploy`, y no se añadió ninguno en esta fase. Para Workers Builds, el comando de compilación previsto es `npm run build`; el comando de preview debe configurarse en Cloudflare como `npx wrangler preview` solo durante la fase autorizada de despliegue temporal.

### Build y rutas

`npm.cmd run build` generó salida estática en `dist/` con:

- `dist/index.html`;
- `dist/servicios/index.html`;
- `dist/portafolio/index.html`;
- `dist/sobre-mi/index.html`;
- `dist/contacto/index.html`.

Se sirvió `dist/` con `astro preview --host 127.0.0.1` y las cinco rutas respondieron `200 text/html`:

`/`, `/servicios/`, `/portafolio/`, `/sobre-mi/`, `/contacto/`.

También respondieron correctamente `favicon.svg`, `robots.txt`, `sitemap.xml` y el logo `/images/nebula-sync/nebula-sync-logo.png`.

### Recursos, SEO y correo

Estado `VERIFIED`:

- favicon presente en `dist/favicon.svg`;
- `robots.txt` presente y referencia `https://diegodebian.online/sitemap.xml`;
- `sitemap.xml` presente;
- canonical, `title`, description y Open Graph presentes en las cinco páginas;
- no se encontraron referencias a `localhost` ni `127.0.0.1` en HTML, XML o texto generado;
- logo auténtico de Nebula Sync presente en `dist/images/nebula-sync/nebula-sync-logo.png`;
- correo comercial presente como `mailto:diegodebian.online@gmail.com`;
- escaneo local de patrones de credenciales sin coincidencias en código y recursos textuales públicos revisados.

No se inspeccionaron binarios como texto durante el escaneo de secretos.

## Etapa B — Cloudflare no verificado

No se dispone de acceso autorizado de solo lectura al Worker existente. Por lo tanto, los siguientes datos quedan `NOT_VERIFIED` y no deben tratarse como valores confirmados:

- si `diegodebian-website` está conectado al repositorio `Diego-debian/diegodebian-website`;
- repositorio y cuenta de GitHub vinculados;
- rama configurada como producción;
- si la rama productiva es `main`;
- si están habilitadas las previews de ramas no productivas;
- si el Worker usa el modelo nuevo de Worker Previews o el modelo anterior;
- comando de build configurado;
- comando de deploy configurado;
- comando de preview configurado;
- directorio raíz configurado;
- variables de build, secrets y bindings de producción o preview;
- dominios `workers.dev`, dominios personalizados y rutas configuradas;
- si un push a la rama actual publicaría automáticamente.

### Pantallas que el usuario debe revisar

En Cloudflare Dashboard:

1. `Workers & Pages` → seleccionar `diegodebian-website` → `Settings` → `Builds` → `Git Repository` → confirmar repositorio y conexión.
2. `Settings` → `Builds` → `Branch control` → confirmar la rama de producción y si están habilitados los builds de ramas no productivas.
3. En la misma pantalla, confirmar `Build command`, `Deploy command`, `Preview command` y `Root directory`.
4. Revisar `Build variables and secrets` y `Preview settings`; no copiar valores privados al repositorio ni al informe.
5. `Workers & Pages` → `diegodebian-website` → `Overview` → confirmar los dominios `workers.dev`, previews existentes, dominios personalizados y rutas.
6. Revisar el historial de builds para determinar qué rama activa producción y qué ramas generan previews.

La documentación oficial indica que los pushes a la rama productiva ejecutan build y deploy, mientras que las ramas no productivas generan previews cuando esa opción está habilitada. También indica que Workers nuevos usan `npx wrangler preview` por defecto y que Workers conectados previamente pueden conservar el modelo anterior hasta una migración explícita: [Build branches](https://developers.cloudflare.com/workers/ci-cd/builds/build-branches/), [Workers Builds configuration](https://developers.cloudflare.com/workers/ci-cd/builds/configuration/) y [Git integration](https://developers.cloudflare.com/workers/ci-cd/builds/git-integration/).

## Riesgo de publicación automática

El riesgo queda `NOT_VERIFIED` para este Worker concreto, pero la consecuencia potencial es clara: si Cloudflare tiene `main` como rama productiva, un push a `main` puede ejecutar el build y el deploy de producción. Si los builds de ramas no productivas están habilitados, un push a `feature/phase-01-project-bootstrap` puede generar una preview; esto no debe asumirse hasta verificar el panel.

La estrategia segura para la siguiente fase es:

1. confirmar que la rama productiva no se cambiará durante la prueba;
2. confirmar que `feature/phase-01-project-bootstrap` se trata como rama no productiva;
3. habilitar o verificar previews sin cambiar el dominio principal;
4. usar `npm run build` como build command y `npx wrangler preview` como preview command, si Cloudflare requiere configuración explícita;
5. revisar el resultado y la URL temporal;
6. no ejecutar `npx wrangler deploy`, no conectar `diegodebian.online` y no tocar DNS;
7. detener la prueba y esperar aprobación antes de cualquier promoción.

## Procedimiento propuesto para una fase posterior

Este procedimiento no se ejecutó en PHASE 06A:

1. Confirmar manualmente los valores de Cloudflare de la Etapa B.
2. Confirmar que el repositorio remoto contiene el commit aprobado `02c47850e28a9564f05c631b4e4404764c056c4f`.
3. Confirmar rama productiva y política de previews antes de enviar cualquier cambio.
4. Si la política exige un push para activar Workers Builds, detenerse y solicitar autorización explícita para ese push.
5. Con autorización de la fase de preview, configurar o usar `npm run build` y `npx wrangler preview` según el panel verificado.
6. Obtener la URL temporal de preview y comprobar las cinco rutas, recursos, SEO y correo.
7. No cambiar el dominio personalizado ni la configuración de producción.
8. Registrar URL, build, rama, commit y resultado en el informe de la fase siguiente.

## Cierre

`PHASE_06A_CLOUDFLARE_PREVIEW_READINESS = PARTIAL`

La parte local está lista y verificada. La fase queda parcial porque la configuración real del Worker, sus triggers y sus dominios requieren revisión autorizada en Cloudflare.

STOP — WAITING FOR EXPLICIT AUTHORIZATION FOR PREVIEW DEPLOYMENT.
