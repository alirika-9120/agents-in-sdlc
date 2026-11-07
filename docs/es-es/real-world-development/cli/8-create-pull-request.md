---
title: "Lección 8: Crear y combinar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de las instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Ahora toca revisarlos en conjunto y abrir una solicitud de incorporación de cambios. Combinaste personalmente la PR de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Lo habitual sería dividir la funcionalidad, las actualizaciones de instrucciones, la actualización de la habilidad y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de filtrado y calidad en una conversación y una rama, y lo incluirás en esta PR.

En esta lección:

- Aprende qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- Inspecciona el cambio completo de la funcionalidad y las pruebas de verificación.
- Crea la PR del filtrado.
- Habilita Agent Merge solo después de la revisión y confirma que se ha combinado la PR.

## Escenario

Durante el flujo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener bajo el control del desarrollador la autorización para combinarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo necesario para integrar una solicitud de incorporación de cambios. Cuando lo habilitas, Copilot resuelve los bloqueos de la PR: corrige las comprobaciones de integración continua (CI) que fallan, responde a los comentarios de revisión y vuelve a aplicar los cambios sobre la rama base cuando hace falta. Después, habilita la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado personalmente **Merge pull request**. Agent Merge puede encargarse de esa responsabilidad. Revisa el trabajo y confirma que está listo antes de habilitar Agent Merge.

## Usa Agent Merge para gestionar la PR

Con todo el código creado, revisa los cambios, crea la PR y deja que Agent Merge gestione el resto del proceso.

1. Vuelve a tu codespace.
2. Abre el cuadro de diálogo del agente con `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todas las comprobaciones se completen correctamente, realizará la combinación.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si el proceso se bloquea, revisa el motivo indicado y completa manualmente la combinación, una vez revisada, cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: generación de código, pruebas y validación y, ahora, gestión de la PR. En esta lección:

- Aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- Inspeccionaste el cambio completo de la funcionalidad y las pruebas de verificación.
- Creaste la PR del filtrado.
- Habilitaste Agent Merge solo después de revisar los cambios y confirmaste que se combinaron.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos y el uso compartido, y para delegar trabajo opcionalmente en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
