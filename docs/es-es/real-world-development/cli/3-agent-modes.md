---
title: "Lección 3: Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos por añadir una pequeña funcionalidad al proyecto. Sin embargo, los cambios más amplios necesitan un proceso más sólido. GitHub Copilot CLI está diseñado para adaptarse al flujo de trabajo existente de una organización y ayudarnos a crear lo adecuado de la manera correcta. En esta primera de varias lecciones seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como se espera y, finalmente, la integrarás en el proyecto.

> [!NOTE]
> Continuarás el flujo de trabajo de esta funcionalidad en la misma conversación y rama. Normalmente se usarían ramas o PR distintas para los diferentes tipos de archivos, pero simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- Inicia una conversación nueva de Copilot a partir de una incidencia de GitHub.
- Define los requisitos en el modo Plan.
- Implementa la funcionalidad con el modo Autopilot.
- Revisa el código.
- Valida manualmente la funcionalidad en un navegador con reenvío de puertos.

En las siguientes lecciones de esta funcionalidad actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys crece y los visitantes necesitan filtrar los juegos por categoría y editor. La incidencia del backlog describe la funcionalidad, pero es necesario acordar algunos detalles, como la combinación de categorías, antes de programar. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia los fundamentos. De hecho, los hace aún más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Parten de una incidencia registrada que describe el trabajo necesario.
2. Crean un plan de lo que hay que desarrollar.
3. Desarrollan y revisan el código.
4. Ejecutan pruebas para validar el código.
5. Validan manualmente la nueva funcionalidad.
6. Crean una solicitud de incorporación de cambios (PR).
7. Cuando se haya revisado el código y se haya completado correctamente la integración continua, combinan el código.

> [!NOTE]
> Los detalles exactos varían según el equipo y la organización, pero la mayoría de los procesos son variaciones de este patrón.

Seguir este enfoque estándar ayuda a garantizar que el código generado por IA cumpla los requisitos y pase por el mismo proceso de validación que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla la autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: trabajas con el agente. Este propone cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma, escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Parte de la incidencia

Antes de empezar a trabajar en el filtrado, vuelve a tu codespace y comprueba que el repositorio y el terminal están listos.

1. Vuelve a tu codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que se ha combinado la PR de las valoraciones por estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de flecha para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia a la solicitud y volver a la pestaña **Session**.

Observa que ahora la solicitud empieza con `#7` (o un número parecido). El carácter `#` permite añadir al contexto una incidencia o una solicitud de incorporación de cambios (PR) de GitHub.

## Planifica la funcionalidad de filtrado

La planificación permite definir cómo se implementará una funcionalidad o se realizará una tarea antes de delegarla en Copilot. Para cualquier trabajo complejo, conviene dedicar un tiempo a planificarlo. Cambia al modo Plan y pide a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo de la solicitud muestra **Plan**.
2. Después de la referencia a la incidencia añadida en el paso anterior, escribe esta solicitud:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot se pone a trabajar en el plan. Primero explorará el proyecto y, después, determinará el mejor enfoque.

3. Es posible que Copilot haga preguntas sobre el funcionamiento del filtrado. Respóndelas según tus preferencias; no hay respuestas incorrectas.
4. Cuando el plan esté listo, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que cambie cualquier parte del plan que no coincida con tus decisiones.

## Aprueba Autopilot

Una vez redactado y revisado el plan, toca implementarlo. Deja que Copilot trabaje con el modo Autopilot.

Autopilot permite que Copilot itere sobre el problema hasta que considere que lo ha resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente en la versión instalada.
2. Confirma que el indicador debajo de la solicitud muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa primero el plan. Si Copilot detecta dependencias ausentes o un conflicto de puertos, resuelve el problema de configuración antes de dar las comprobaciones por completadas.

## Revisa y verifica la implementación

Una vez generado el código, debes revisarlo antes de combinarlo, como harías con cualquier otro cambio. Revisa el código y ejecuta el sitio para comprobar que todo funciona correctamente.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones tomadas durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que resuelva los fallos.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría, el filtrado por editor y las combinaciones acordadas en el plan.
4. Confirma que el restablecimiento y el comportamiento sin resultados coinciden con la incidencia y tus decisiones.
5. Vuelve a tu codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y siguientes pasos

Has usado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- Iniciaste una conversación nueva de Copilot a partir de una incidencia de GitHub.
- Definiste los requisitos en el modo Plan.
- Implementaste la funcionalidad con el modo Autopilot.
- Revisaste el código.
- Validaste manualmente la funcionalidad en un navegador con reenvío de puertos.

A continuación, profundizarás en cómo generar código que siga las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
