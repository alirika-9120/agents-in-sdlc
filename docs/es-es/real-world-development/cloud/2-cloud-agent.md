---
title: "Ejercicio 2: Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Es probable que casi todas las organizaciones tengan dificultades para gestionar la deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que hay que actualizar o solicitudes de funcionalidades que siguen en el backlog por falta de tiempo para implementarlas. El agente de GitHub Copilot en la nube está diseñado para realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea un borrador de solicitud de incorporación de cambios (PR) para que lo revise una persona. Así puedes delegar tareas tediosas, acelerar el desarrollo y dedicarte a cuestiones de mayor alcance.

Con el agente de Copilot en la nube explorarás cómo:

- Personalizar el entorno de generación de código.
- Garantizar que las operaciones se realicen de forma segura.
- Crear incidencias con un alcance claro.
- Asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: de hecho, no hay documentación. El primer paso es añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la forma de descubrir juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para que los usuarios puedan seguir explorando. Todavía no hace falta una implementación pulida; solo necesitan algo con lo que probar y aceptar la experiencia de usuario (UX). Aunque ahora mismo es un bloqueo, hay otras incidencias con más prioridad.

Estas tareas pueden quedar relegadas fácilmente a un segundo plano, por lo que son buenas candidatas para asignarlas al agente de Copilot en la nube. El agente puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, después, revisar su trabajo y confirmar que cumple las expectativas.

## Introducción al agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Puedes iniciar el trabajo de varias maneras, por ejemplo, [asignando una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Una vez asignada, Copilot crea un borrador de solicitud de incorporación de cambios para registrar el progreso, configura el entorno y empieza a trabajar. Puedes consultar la sesión mientras sigue en curso o una vez completada. Cuando la solución propuesta esté lista para revisión, Copilot te mencionará en la solicitud de incorporación de cambios.

## La importancia de definir bien el alcance de las instrucciones

GitHub Copilot no hace magia: no puedes saltarte el trabajo de pensar. Incluso las tareas aparentemente sencillas tienen cierta complejidad cuando se analizan en detalle, así que [define cuidadosamente el alcance de las tareas para el agente de Copilot en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al incorporar IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has aprendido qué son las instrucciones personalizadas y cómo orientan a Copilot. Si ya completaste el ejercicio práctico de instrucciones personalizadas, habrás visto cómo funciona `.github/copilot-instructions.md`. Si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente de Copilot en la nube, revisa los archivos de instrucciones que incluye el repositorio para poder observar su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un codespace que ya esté en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las expectativas sobre las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que limita estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la siguiente sección, busca comentarios de documentación TSDoc, convenciones de TypeScript y comentarios de cabecera en la solicitud de incorporación de cambios resultante. Proceden de los archivos de instrucciones que has revisado. Volveremos a señalarlo al revisar las PR en un ejercicio posterior.

## Configura el entorno de desarrollo del agente de Copilot en la nube

Crear código, independientemente de quién lo haga, suele requerir un entorno específico y algunos scripts de configuración para garantizar que todo esté listo. Lo mismo ocurre cuando asignas tareas a Copilot, que trabaja de forma parecida a un ingeniero de software (SWE).

El agente en la nube utiliza [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo mediante un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece a trabajar. Así puede acceder a las herramientas de desarrollo y dependencias necesarias. Este flujo ya está configurado en la plantilla del taller para facilitar los ejercicios y permitirte aprender cómo funciona. Proporciona a Copilot Node.js, las dependencias del proyecto, los navegadores necesarios para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Configura los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para las operaciones que realice.
    permissions:
      # Si quieres clonar el repositorio durante los pasos de configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no clonas el repositorio en los pasos de configuración, Copilot lo hará automáticamente cuando estos terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de frontend y aplicación: Node.js (la aplicación ahora es Astro con Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y precarga la base de datos SQLite local para que las compilaciones y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- Observa el desencadenador `workflow_dispatch`, que permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions** del repositorio. Es útil para comprobar que se ejecuta correctamente sin esperar a que Copilot lo inicie.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de documentación. Este tipo de deuda técnica suele quedar desatendida, reduce la productividad y dificulta mantener el código base o incorporar nuevos desarrolladores al equipo. Copilot puede ayudar a crear documentación, por lo que esta es una incidencia ideal para asignarle al agente de Copilot en la nube. El agente trabajará en segundo plano y generará la documentación necesaria. En un ejercicio posterior volverás para revisar el trabajo.

1. Abre tu repositorio en github.com en una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. En **Description**, escribe:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

   ![Detalles de la asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios (PR) recién generada, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> Es posible que la lista de tareas de Copilot tarde varios minutos en aparecer en la PR. Copilot está preparando su entorno (ejecutando el flujo de trabajo mencionado), analizando el proyecto y determinando el mejor enfoque.

13. Revisa la lista y las tareas que realizará.
14. Desplázate hacia abajo por la cronología de la solicitud de incorporación de cambios. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa que puedes desplazarte por la sesión en directo y ver cómo Copilot resuelve el problema: explora el código y comprende su estado, se detiene a analizar y elegir un plan y, después, crea código.

Es probable que este proceso tarde varios minutos. Uno de los objetivos principales del agente de Copilot en la nube es realizar tareas de forma asíncrona para que podamos centrarnos en otras actividades. Aprovecharemos precisamente esa función: asignaremos otra tarea al agente en la nube y, mientras tanto, nos ocuparemos de desarrollar una funcionalidad.

## Añade una sección de juegos relacionados a la página de detalles

Una de las ventajas del agente de Copilot en la nube es que permite dividir el trabajo: puedes centrarte en un conjunto de tareas mientras el agente se ocupa de otro. Aunque añadir una sección de juegos relacionados para el equipo de diseño no debería llevar demasiado tiempo, podrías aprovechar ese tiempo para otras tareas. Asigna el trabajo al agente en la nube.

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en el campo **Title**.
6. En **Description**, escribe:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, aparecerán varios emojis 👀 en el primer comentario de la incidencia para indicar que Copilot ya está trabajando.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. Como el agente en la nube trabaja de forma parecida a un ingeniero de software, no tienes que supervisarlo constantemente; puedes revisar su trabajo cuando lo complete. Ahora, centrémonos en crear y utilizar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona. Copilot puede ayudarte con la deuda técnica, crear funcionalidades o migrar código de un framework a otro.

Has aprendido:

- A personalizar el entorno para generar código.
- A garantizar que las operaciones se realizan de forma segura.
- La importancia de definir con claridad el alcance de las incidencias.
- A asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes crear y utilizar agentes personalizados. El [agente de Copilot en la nube también puede usar servidores MCP][cloud-agent-mcp] y recibe instrucciones personalizadas, que ya exploramos en módulos anteriores.

## Recursos

- [Acerca del agente de Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
