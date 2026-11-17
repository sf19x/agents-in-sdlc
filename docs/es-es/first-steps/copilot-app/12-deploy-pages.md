---
title: "Lección 12 - Desplegar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de incorporación de cambios y verificar el cuestionario espacial publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica el cuestionario espacial en Internet con GitHub Pages. Copilot puede preparar el flujo de trabajo y guiar el despliegue, pero tú debes revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- pedirás a Copilot que configure un despliegue en GitHub Pages.
- revisarás el flujo de trabajo y la solicitud de incorporación de cambios antes de combinarla.
- seguirás el despliegue y verificarás el sitio publicado.

## Escenario

El cuestionario espacial ya está en un repositorio de GitHub, pero hace falta una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages lo hace accesible mediante una URL pública, sin añadir un servidor ni dependencias de ejecución.

## Pide a Copilot que prepare el despliegue

> [!WARNING]
> Crear una solicitud de incorporación de cambios puede confirmar y enviar código. Revisa la configuración de aprobación y el repositorio de destino antes de enviar este prompt. Con **Approve all**, no esperes otro aviso de confirmación.

En la sesión `space-quiz` de la aplicación Copilot, envía este prompt:

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## Revisa el flujo de trabajo y la solicitud de incorporación de cambios

1. Confirma que el archivo README solo se añade si el repositorio todavía no tiene uno.
2. Revisa los cambios de la solicitud, incluidos los pasos de compilación y despliegue del flujo de trabajo, los desencadenadores y los permisos.
3. Confirma que el flujo despliega desde la rama predeterminada, no desde una solicitud de incorporación de cambios sin combinar.
4. Solicita las correcciones necesarias y revisa el diff actualizado. Espera a que se superen las comprobaciones obligatorias antes de autorizar la combinación.

## Autoriza la combinación y el despliegue

> [!WARNING]
> El prompt siguiente autoriza al agente a combinar la solicitud revisada, cambiar la configuración de Pages del repositorio y publicar el sitio. Envíalo solo después de revisar los cambios y confirmar el repositorio y la solicitud. La configuración de aprobación puede permitir estas acciones sin otro aviso.

Sustituye `<PR_NUMBER>` por el número de la solicitud revisada y envía este prompt por separado:

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama de tres etapas: una solicitud de incorporación de cambios de space-quiz combinada; una ejecución correcta de Deploy website en GitHub Actions con los pasos Build website, Upload artifact y Deploy to Pages; y el cuestionario espacial publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Verifica el despliegue

1. Sigue la ejecución de despliegue de GitHub Actions hasta que finalice correctamente.
2. Abre la URL indicada y confirma que se carga el cuestionario espacial.

Si el agente no puede habilitar Pages o combinar la solicitud, sigue las instrucciones concretas que te proporcione. No des la tarea por terminada hasta que el despliegue se complete y el sitio publicado se cargue.

## Resumen y siguientes pasos

El cuestionario espacial ya está disponible como sitio publicado y has verificado el despliegue en lugar de confiar solo en el informe del agente. En esta lección:

- has pedido a Copilot que configure un despliegue en GitHub Pages.
- has revisado el flujo de trabajo y la solicitud de incorporación de cambios antes de combinarla.
- has seguido el despliegue y verificado el sitio publicado.

Continúa con [la lección 13: Revisión y siguientes pasos][next-lesson].

[next-lesson]: ../13-review/
