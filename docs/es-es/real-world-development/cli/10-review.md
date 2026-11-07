---
title: "Lección 10: Repaso y siguientes pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI en un flujo continuo de Tailspin Toys. En este taller:

- Preparaste un codespace, instalaste Copilot CLI, exploraste el proyecto y localizaste la incidencia de filtrado del backlog.
- Añadiste valoraciones por estrellas, revisaste el resultado en un navegador con reenvío de puertos y combinaste manualmente tu primera solicitud de incorporación de cambios (PR).
- Partiste de la incidencia de filtrado, definiste el enfoque en modo Plan, lo implementaste en modo Autopilot y lo revisaste en modo Interactive.
- Orientaste al agente con instrucciones personalizadas, personalizaste la habilidad `quality-checks` existente y la usaste para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- Añadiste el servidor Model Context Protocol (MCP) de Playwright y lo usaste para explorar el filtrado en un navegador real.
- Creaste y seleccionaste un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador.
- Revisaste el cambio completo del filtrado y autorizaste Agent Merge para su PR.
- Exploraste comandos de barra para gestionar el contexto, los modelos y el uso compartido, y para delegar trabajo opcionalmente en la nube.

## Lo que has entregado

El taller incluye dos hitos de PR, cada uno en su propia rama creada a partir de `main` actualizado:

1. **Valoraciones por estrellas**: muestra en las tarjetas de juegos `starRating` y un estado explícito para los juegos sin valorar.
2. **Filtrado y flujo de calidad**: implementa el filtrado, actualiza las instrucciones y aplícalas a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la apertura de su PR, trabajaste en la misma conversación y rama. Combinamos todo ese trabajo en una PR para simplificar el taller.

## Distintos tipos de verificación

Comprobaste el código de varias formas: con pruebas automatizadas, inspeccionándolo en el navegador y dejando que Copilot lo explorase mediante MCP. La habilidad `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos, y comunicó los resultados con el nuevo formato. QA combinó esos resultados con una revisión de los requisitos y la cobertura antes de abrir la PR.

Las pruebas nuevas deben cubrir carencias reales; también puede ser correcto que una ejecución de QA no requiera pruebas nuevas. Revisa el código y las pruebas antes de autorizar la combinación, y actualiza las pruebas de verificación y las evidencias afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller actualizaste instrucciones, personalizaste una habilidad, creaste un perfil de QA y configuraste un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y ajústalas a las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios reales y los resultados de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** según la tarea. Usa **Plan** para pensar en un enfoque antes de implementarlo, **Interactive** para seguir de cerca los cambios acotados y **Autopilot** para las tareas bien delimitadas. Elige un modelo más rápido para las modificaciones rutinarias y uno más avanzado para el trabajo complejo.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* influye de forma importante en el resultado.

## Más funciones para explorar

Ya has completado el flujo de trabajo principal. Estas son algunas funciones más de la CLI que merece la pena explorar:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar una tarea distinta.
- `/delegate` para enviar una tarea al agente de Copilot en la nube.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola: para código de producción, proyectos personales o esa pequeña aplicación que llevas años queriendo crear. Comparte lo que aprendas con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Backer Concierge con Foundry][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
