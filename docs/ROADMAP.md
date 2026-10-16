# Roadmap

## PHASE_00_DISCOVERY_AND_ARCHITECTURE

Objetivo: recuperar el repositorio, documentar arquitectura, alcance, riesgos y criterios.

Estado: completada mediante esta documentación. No incluye código de aplicación.

## PHASE_01_PROJECT_BOOTSTRAP

Objetivo: preparar Astro, TypeScript, scripts, estructura de páginas y configuración mínima.

Archivos previstos: `package.json`, lockfile, `astro.config.*`, `tsconfig.json`, `.gitignore`, `src/` y `public/`.

Dependencias: autorización expresa, revisión del repositorio y selección final de versiones.

Pruebas: instalación reproducible y compilación local.

Ramas: trabajar en `feature/phase-01-*`; mantener `main` protegida y sin cambios directos. No hacer `push` hasta revisar la configuración de Workers Builds.

Punto de detención: no continuar al diseño sin revisión del bootstrap.

## PHASE_02_DESIGN_SYSTEM

Objetivo: implementar la propuesta visual A, responsive, accesible y con soporte de tema si no añade complejidad innecesaria.

Riesgos: contraste insuficiente, exceso de animación o identidad visual genérica.

Punto de detención: revisión visual antes de cargar el contenido final.

## PHASE_03_CONTENT_AND_PORTFOLIO

Objetivo: incorporar contenido autorizado y Nebula Sync como único caso presentado.

Riesgos: convertir competencias en afirmaciones no verificadas o publicar datos personales.

Punto de detención: aprobación del contenido comercial.

## PHASE_04_TESTING_AND_HARDENING

Objetivo: validar build, tipos, enlaces, accesibilidad, SEO, rutas, responsive, seguridad y ausencia de secretos.

Punto de detención: no declarar PASS para verificaciones no ejecutadas.

## PHASE_05_CLOUDFLARE_PREVIEW

Objetivo: configurar una preview temporal y comprobar el comportamiento sin servicios locales.

Dependencias: Worker y Workers Builds confirmados.

Comandos previstos: `npm run build` para compilar y `npx wrangler preview` para una rama no productiva. El comando `npx wrangler deploy` queda reservado para la rama productiva y una fase autorizada.

Punto de detención: no tocar el dominio principal.

## PHASE_06_CUSTOM_DOMAIN_MIGRATION

Objetivo: asociar el dominio principal únicamente con autorización expresa.

Riesgos: afectar registros existentes o servicios de GestionCore.

Punto de detención: respaldo y revisión del plan DNS antes de cualquier cambio.

## PHASE_07_POST_DEPLOYMENT_AUDIT

Objetivo: revisar producción, rendimiento, indexación, headers, rutas y regresiones.

Punto de detención: documentar cualquier diferencia entre lo esperado y lo observado.
