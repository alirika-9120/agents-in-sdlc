---
title: "Lección 12: Implementar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de incorporación de cambios y verificar el Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica el Space Quiz para que cualquiera pueda probarlo. Copilot puede preparar el flujo de trabajo y guiar la implementación, pero tú eres responsable de revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- Pide a Copilot que configure una implementación con GitHub Pages.
- Revisa el flujo de trabajo y la solicitud de incorporación de cambios antes de combinarla.
- Sigue la implementación y verifica el sitio publicado.

## Escenario

El Space Quiz ya está en un repositorio de GitHub, pero la gente necesita una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages permite acceder a él desde una URL pública sin añadir un servidor ni una dependencia de entorno de ejecución.

## Pide a Copilot que implemente el sitio

En la sesión de la aplicación Copilot para `space-quiz`, envía esta solicitud:

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama en tres etapas: una solicitud de incorporación de cambios de space-quiz combinada; una ejecución correcta de Deploy website en GitHub Actions con los pasos Build website, Upload artifact y Deploy to Pages; y el Space Quiz activo en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Esta solicitud pide al agente que cree y combine una solicitud de incorporación de cambios y cambie la configuración de Pages del repositorio. Revisa la diferencia, los permisos de implementación y cualquier solicitud de aprobación de GitHub antes de continuar.

## Revisa el flujo de trabajo y verifica la implementación

1. Confirma que el archivo README solo se añade si el repositorio aún no tiene uno.
2. Revisa los cambios de la solicitud de incorporación de cambios, incluidos los pasos de compilación e implementación del flujo de trabajo.
3. Combina los cambios únicamente cuando se superen las comprobaciones obligatorias de la solicitud.
4. Sigue la ejecución de implementación de GitHub Actions hasta que se complete correctamente.
5. Abre la URL indicada y confirma que se carga el Space Quiz.

Si el agente no puede habilitar Pages ni combinar la solicitud de incorporación de cambios, sigue las instrucciones específicas que te proporcione. No des la tarea por terminada hasta que la implementación se complete y el sitio esté disponible.

## Resumen y siguientes pasos

El Space Quiz ya está disponible como sitio activo y has verificado la implementación en lugar de confiar solo en el informe del agente. Continúa con la [Lección 13: Revisión y siguientes pasos][next-lesson].

[next-lesson]: ../13-review/
