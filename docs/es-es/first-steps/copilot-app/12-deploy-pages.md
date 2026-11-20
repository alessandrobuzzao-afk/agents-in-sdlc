---
title: "Lección 12 - Despliega el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de incorporación de cambios y comprobar el Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dale un hogar público al Space Quiz desplegándolo con GitHub Pages. Copilot puede preparar el flujo de trabajo y guiar el despliegue, pero tú sigues siendo responsable de revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- pedirás a Copilot que configure un despliegue con GitHub Pages;
- revisarás el flujo de trabajo y la solicitud de incorporación de cambios antes de combinarlos;
- seguirás el despliegue y comprobarás el sitio publicado.

## Escenario

El Space Quiz ya está en un repositorio de GitHub, pero otras personas aún necesitan una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages proporciona una URL pública sin añadir un servidor ni dependencias de ejecución.

## Pide a Copilot que prepare el despliegue

> [!WARNING]
> Crear una solicitud de incorporación de cambios puede confirmar y enviar código. Revisa la configuración de aprobación y el repositorio de destino antes de enviar este prompt. Si está seleccionada la opción **Approve all**, no esperes otro aviso de confirmación.

En la sesión de la aplicación Copilot para `space-quiz`, envía este prompt:

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## Revisa el flujo de trabajo y la solicitud de incorporación de cambios

1. Confirma que el archivo README solo se añade si el repositorio aún no tiene uno.
2. Revisa los cambios de la solicitud, incluidos los pasos de compilación y despliegue del flujo de trabajo, sus desencadenadores y permisos.
3. Confirma que el flujo de trabajo despliega desde la rama predeterminada, no desde una solicitud de incorporación de cambios que aún no se haya combinado.
4. Solicita las correcciones necesarias y revisa el diff actualizado. Espera a que se superen las comprobaciones obligatorias de la solicitud antes de autorizar su combinación.

## Autoriza la combinación y el despliegue

> [!WARNING]
> El siguiente prompt autoriza al agente a combinar la solicitud revisada, cambiar la configuración de Pages del repositorio y publicar el sitio. Envíalo solo después de completar la revisión y confirmar el repositorio y la solicitud. La configuración de aprobación podría permitir estas acciones sin pedir otra confirmación.

Sustituye `<PR_NUMBER>` por el número de la solicitud que has revisado y envía este prompt por separado:

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama de tres fases: una solicitud de incorporación de cambios de space-quiz combinada; una ejecución correcta de Deploy website en GitHub Actions con los pasos Build website, Upload artifact y Deploy to Pages; y el Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Comprueba el despliegue

1. Sigue la ejecución de despliegue de GitHub Actions hasta que se complete correctamente.
2. Abre la URL indicada y confirma que se carga el Space Quiz.

Si el agente no puede habilitar Pages o combinar la solicitud, sigue las instrucciones específicas que te proporcione. No des la tarea por terminada hasta que el despliegue finalice correctamente y el sitio esté disponible.

## Resumen y próximos pasos

Ahora tu Space Quiz está disponible como sitio web y has comprobado el despliegue en lugar de confiar únicamente en el informe del agente. Continúa con la [Lección 13: Repaso y próximos pasos][next-lesson].

[next-lesson]: ../13-review/
