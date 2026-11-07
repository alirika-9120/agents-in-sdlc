---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Supervisar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidad. Pediste a Copilot que documentara el código, creara una funcionalidad de juegos relacionados que el equipo de diseño pudiera evaluar e implementara mejoras de accesibilidad con selectores de modo de alto contraste y claro. Ahora revisarás los cambios de código y, si hace falta, enviarás comentarios para que Copilot mejore su trabajo.

### Escenario

Como hemos señalado varias veces, los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Con esto en mente, revisa las propuestas de GitHub Copilot para documentar el código, añadir juegos relacionados y mejorar la accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y el agente de GitHub Copilot en la nube

Como el agente de Copilot en la nube realiza tareas de forma asíncrona y sin supervisión, se han establecido restricciones de seguridad para que el proceso siga siendo seguro:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utilizará para su código.
- El agente en la nube se ejecuta en GitHub Actions, donde crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que se ejecuten.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empieza por explorar la primera solicitud de incorporación de cambios (PR) creada por el agente de Copilot en la nube, que añade documentación al código. Utilizarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Al explorar la PR, quizá veas una advertencia que indica que el cortafuegos ha bloqueado GitHub Copilot. **Es normal**: Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el cortafuegos del agente de Copilot en la nube][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la solicitud de incorporación de cambios con un título parecido a **Add missing documentation** o uno más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud tendrá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos. Mientras tanto, puedes descansar o repasar lo aprendido.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que ahora incluye comentarios de documentación TSDoc y otra documentación. Los cambios exactos variarán.

   Busca comentarios de documentación TSDoc, convenciones de TypeScript y comentarios de cabecera. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del Ejercicio 2.

6. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que algunos flujos de trabajo esperan aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto, que es una aplicación Astro única, se completarán correctamente. Pueden tardar unos minutos.

## Pide cambios a GitHub Copilot

Trabajar con Copilot en una solicitud de incorporación de cambios no es un proceso unidireccional. Puedes mencionarlo en comentarios de la PR, igual que harías con otras personas del equipo, o añadir comentarios en línea en el código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos recomendar un texto exacto. Puedes pedir a Copilot que:

- Añada al principio de cada archivo de código un comentario breve que explique su función.
- Añada comentarios de documentación TSDoc a archivos TypeScript y Astro.
- Cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario con una solicitud de cambio a la documentación generada y menciona a **@copilot**, como harías con cualquier usuario. Usa una de las ideas anteriores u otra petición sobre la documentación del código base.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una sesión nueva para realizar las actualizaciones.
3. Selecciona **Back to pull request** para volver a la solicitud.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, debería aparecer una confirmación nueva en la PR.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Cuando lo estés, puedes marcar la PR como lista para revisión y combinarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Ahora vuelve a la PR que Copilot creó para resolver la incidencia de mostrar juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page** o uno más descriptivo.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que señala que algunos flujos de trabajo esperan aprobación.
7. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto, que es una aplicación Astro única, se completarán correctamente. Pueden tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en tu codespace para probar manualmente la funcionalidad de juegos relacionados. Ve al codespace, abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ya ha creado la funcionalidad de juegos relacionados. Como antes, puedes iterar con el agente de Copilot en la nube y solicitar actualizaciones, por ejemplo, cambiar el número de juegos relacionados o añadir comentarios de cabecera y comentarios de documentación TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Para solicitar cambios, añade un comentario en la pestaña **Conversation**; Copilot lo verá e iniciará una sesión nueva.

## Revisa las funcionalidades de accesibilidad

Por último, revisa las funcionalidades de accesibilidad implementadas por el agente personalizado de accesibilidad. Esta PR debería incluir el modo de alto contraste que asignaste en el Ejercicio 3 y el modo claro que solicitaste desde el panel de agentes en el Ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR con un título parecido a **Add high contrast mode to website** o uno más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud tendrá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - Los componentes de la interfaz para cambiar entre modos.
   - El uso del almacenamiento local para guardar las preferencias del usuario.
   - Los cambios de CSS o de estilos para los modos de alto contraste y claro.
   - Los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado.
   - El código JavaScript o TypeScript que gestiona los cambios de modo.

6. Cuando hayas revisado las actualizaciones y te parezcan correctas, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que algunos flujos de trabajo esperan aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto, que es una aplicación Astro única, se completarán correctamente. Pueden tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu codespace para probar manualmente las funcionalidades de accesibilidad. Ve al codespace, abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Después, inicia la aplicación y prueba los selectores de los modos de alto contraste y claro en el navegador. Confirma que funcionan y que las preferencias se conservan al volver a cargar las páginas.

Observa cómo el agente personalizado de accesibilidad orientó a Copilot para implementar estas funcionalidades según las prácticas recomendadas de accesibilidad. Si detectas problemas o mejoras posibles, menciona a **@copilot** en un comentario y solicita los cambios, como hiciste con las PR anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil cuando puedes asignarle incidencias reales del backlog y ocuparte de otra cosa mientras trabaja. Para practicar este enfoque, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Por ejemplo:

- Crear un formulario para que los patrocinadores muestren interés en la página de detalles del juego.
- Implementar paginación en la página con la lista de juegos.
- Añadir validación de entradas y gestión de errores a las funciones de acceso a datos de `src/lib/`.

## Resumen

Has completado el itinerario del agente en la nube. En estas lecciones:

- **Inspeccionaste los archivos de instrucciones personalizados del repositorio** para observar su efecto en el trabajo del agente en la nube.
- **Asignaste incidencias al agente de Copilot en la nube** y observaste cómo configura el entorno, planifica y trabaja de forma asíncrona.
- **Creaste y utilizaste un agente personalizado** para mejorar la accesibilidad con selectores de los modos de alto contraste y claro.
- **Usaste la página de agentes de Copilot como centro de control** para supervisar y orientar durante su ejecución la sesión de accesibilidad.
- **Revisaste e iteraste sobre las solicitudes de incorporación de cambios del agente en la nube**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Repaso y siguientes pasos

Has completado el itinerario del agente en la nube. Si quieres seguir explorando, los demás itinerarios complementan lo que has practicado:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración MCP directamente desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de QA, MCP de Playwright, Agent Merge y el comando `/delegate` para volver opcionalmente al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente en la nube una incidencia de refactorización o de cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente de Copilot en la nube][agent-firewall]

---

| [← Lección anterior: Supervisar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
