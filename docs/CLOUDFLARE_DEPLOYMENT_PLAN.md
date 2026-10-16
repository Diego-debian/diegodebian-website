# Cloudflare Deployment Plan

Fecha: 2026-10-14

## Alcance

Este documento describe el plan, no ejecuta ningún despliegue. La publicación queda fuera de PHASE_00R.

## Flujo previsto

1. Astro compila el sitio con `npm run build`.
2. La compilación genera `dist/`.
3. Wrangler configura Workers Static Assets con `assets.directory = "./dist"`.
4. Workers Builds ejecuta `npm run build` y, para producción, `npx wrangler deploy` únicamente desde la rama productiva configurada.
5. La primera validación se hace en una preview de Workers, usando `npx wrangler preview` en ramas no productivas, sin migrar el dominio principal.

Este es el flujo de Workers. No se usarán `wrangler pages deploy`, `wrangler pages dev`, `pages_build_output_dir` ni la configuración antigua de Workers Sites.

## Configuración por confirmar

- Nombre exacto del Worker.
- Rama productiva.
- Comando de build configurado en Cloudflare.
- Comando de deploy configurado en Cloudflare.
- `compatibility_date`.
- Política de previews.
- Si el Worker existente usa el modelo anterior de previews; Cloudflare indica que algunos Workers conectados previamente a Builds conservan ese modelo hasta una migración explícita.
- Existencia de una configuración Wrangler previa.
- Reglas de dominio y certificados.
- Si el Worker usa actualmente un script `main`; para este sitio estático no se propone añadirlo.

## Recomendación inicial

Para un sitio estático, evaluar una configuración mínima equivalente a:

```jsonc
{
  "$schema": "./node_modules/wrangler/config-schema.json",
  "name": "diegodebian-website",
  "compatibility_date": "FECHA_A_CONFIRMAR",
  "assets": {
    "directory": "./dist"
  }
}
```

El archivo final no debe crearse hasta iniciar la fase de bootstrap y confirmar la configuración real del Worker.

## Prevención de despliegues accidentales

- Mantener `main` como rama de producción protegida y sin trabajo directo.
- Implementar cada cambio en ramas `feature/phase-01-*` y revisar mediante pull request.
- Usar una rama `integration` solo si se necesita integrar varios cambios antes del pull request final.
- Configurar el trigger de preview para ramas distintas de `main`, con `npm run build` y `npx wrangler preview`.
- Configurar el trigger productivo para `main`, con `npm run build` y `npx wrangler deploy`.
- Revisar el branch control de Workers Builds antes de conectar despliegues automáticos.
- No hacer `push` sin autorización expresa.
- No ejecutar `wrangler deploy` en PHASE_00R ni PHASE_01.

## Dominio y servicios protegidos

La migración de `diegodebian.online` queda para `PHASE_06`. El registro A histórico de `84.32.84.32` solo se documentará. No se tocarán DNS, `gestioncore-ga10`, `gestioncore.diegodebian.online`, certificados ni tareas programadas.

## Fuentes técnicas

- Astro: https://docs.astro.build/en/guides/deploy/
- Astro Cloudflare: https://docs.astro.build/en/guides/integrations-guide/cloudflare/
- Cloudflare Static Assets: https://developers.cloudflare.com/workers/static-assets/
- Cloudflare Git integration: https://developers.cloudflare.com/workers/ci-cd/builds/git-integration/
- Cloudflare custom domains: https://developers.cloudflare.com/workers/configuration/routing/custom-domains/
- Cloudflare build branches: https://developers.cloudflare.com/workers/ci-cd/builds/build-branches/
