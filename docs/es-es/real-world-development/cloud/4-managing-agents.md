---
title: "Ejercicio 4: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores pediste al agente de Copilot en la nube que realizara tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidad. Aunque el agente en la nube trabaja de forma autónoma y asíncrona, es importante supervisar estas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, como la [página de agentes][agents-page] de GitHub.com. Este centro de control muestra todas las tareas de los agentes que tienen solicitudes de incorporación de cambios (PR) abiertas. Puedes explorar las operaciones realizadas e incluso orientar una sesión en curso.

En esta lección:

- Explorarás la página de agentes para supervisar las tareas del agente en la nube.
- Orientarás una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la implementación de un modo de alto contraste, el equipo decidió que sería un buen momento para añadir también un modo claro. Como ya se estaba actualizando el estilo del sitio e implementando un selector, parecía lógico incluir esta funcionalidad. Orientarás el trabajo del agente para que añada tanto el modo claro como el de alto contraste.

## Revisa las tareas del agente de Copilot en la nube

Consulta el estado actual de todas las tareas asignadas al agente de Copilot en la nube:

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Revisa la lista de tareas, tanto en el panel principal como en el panel de la izquierda. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - Actualizar la documentación del código base.
   - Generar API para modificar juegos.
   - Añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa el trabajo que ha realizado Copilot, que puede incluir:
   - Obtener el código del repositorio.
   - Crear el entorno de trabajo de Copilot.
   - Configurar servidores MCP.
   - Realizar los distintos pasos necesarios para completar la tarea asignada.

> [!NOTE]
> Los pasos exactos varían según el estado del trabajo de Copilot y el enfoque utilizado.

4. Observa también el panel de solicitudes de incorporación de cambios que aparece a la derecha. Te permite ver la PR y los archivos modificados para seguir el trabajo.

## Orienta al agente en la nube

Ya has visto las tareas activas. Ahora pide a Copilot que añada un selector de modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con la implementación del modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

   ![Sesión de accesibilidad en el panel de agentes](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos hasta que indique que ha terminado de configurar el entorno y ha empezado a trabajar. Lo sabrás cuando empieces a ver mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe esta solicitud:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Captura de pantalla de la tarea del agente en la página de agentes, con el cuadro de diálogo para orientar la sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar la solicitud.
5. Observa cómo Copilot la confirma y la incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot se ocupará de la tarea actualizada. Incorporará la nueva solicitud al flujo de trabajo cuando termine el paso que estaba ejecutando en el momento de enviarla.

El proceso tardará varios minutos. Es un buen momento para hacer una pausa y repasar lo aprendido hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y orientar las tareas del agente de GitHub Copilot en la nube. Permite seguir las tareas activas y completadas, revisar el trabajo realizado y redirigir tareas en curso para ajustar el alcance u ofrecer nuevas indicaciones.

Has aprendido:

- A usar el panel de agentes para supervisar las tareas del agente en la nube.
- A redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina de trabajar en las funcionalidades de accesibilidad, [revisarás las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
