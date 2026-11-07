---
title: "Ejercicio 3: Probar la funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo agente. Antes de abrir una solicitud de incorporación de cambios, comprueba que funciona en el navegador. En lugar de probar la aplicación manualmente, conectarás el **servidor MCP de Playwright** y permitirás que Copilot use un navegador real para probar la funcionalidad. Después, publicarás la rama y abrirás la PR.

En este ejercicio:

- Aprenderás qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot.
- Añadirás el servidor MCP de Playwright al espacio de trabajo.
- Pedirás a Copilot que lo use para probar manualmente la funcionalidad de filtrado en un navegador.
- Publicarás la rama y abrirás una solicitud de incorporación de cambios para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo agente es mucho más potente cuando puede acceder a recursos externos al editor. Copilot lo consigue mediante Model Context Protocol (MCP), un estándar que permite al agente comunicarse con herramientas y servicios externos.

![Diagrama que muestra el funcionamiento interno del modo agente y cómo interactúa con el contexto, el LLM y las herramientas, incluidas las que proporcionan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada (mediante recursos) y realizar acciones en tu nombre (mediante herramientas).

Los agentes acceden a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. El servidor MCP gestiona la comunicación con las herramientas externas, como las API existentes o las herramientas locales, por ejemplo, los paquetes de NPM. Cada servidor MCP representa un conjunto distinto de herramientas y recursos a los que puede acceder el agente.

Algunos servidores MCP conocidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a varias API para gestionar repositorios de GitHub. Permite al agente de IA crear repositorios, actualizar los existentes y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: ofrece funciones de automatización del navegador con Playwright. Permite al agente navegar por páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP que proporcionan acceso a distintas herramientas y recursos. GitHub aloja un [registro de MCP](https://github.com/mcp) que facilita descubrir servidores y contribuir al ecosistema.

> [!CAUTION]
> Por motivos de seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar un servidor MCP, revisa detenidamente su código fuente, verifica quién lo publica y ten en cuenta las implicaciones de seguridad. Usa únicamente servidores MCP de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu codespace.
2. Deberías ver que ya hay un servidor `github` configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

La plantilla del proyecto incluye la entrada `github`; por eso Copilot pudo leer el backlog de incidencias en el ejercicio anterior. Utiliza el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hay que instalarlo localmente. VS Code se conecta por HTTP y te autenticas con GitHub la primera vez que Copilot utiliza una de sus herramientas.

## Añade el servidor MCP de Playwright

Ahora añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar, justo lo que necesitas para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en tu equipo mediante `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin ventana visible, algo necesario en un codespace, que no dispone de un escritorio donde mostrarlo.

> [!NOTE]
> El proyecto Tailspin Toys ya utiliza Playwright para las pruebas de extremo a extremo, por lo que normalmente ya está instalado el navegador que necesita Playwright. Si Copilot indica más adelante que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Inicia el servidor y confirma que es de confianza

VS Code inicia los servidores MCP **cuando se necesitan**. La primera vez que Copilot requiere las herramientas de un servidor, VS Code lo inicia y te pregunta si confías en él.

1. En `.vscode/mcp.json`, verás una acción **Start** sobre la entrada `playwright`. Selecciónala para iniciar el servidor y comprobar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en ella para que se pueda iniciar el servidor.

Iniciarlo manualmente es opcional. Si omites este paso, VS Code lo iniciará y te pedirá que confirmes que confías en él la primera vez que Copilot necesite sus herramientas.

## Prueba la funcionalidad de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador real. En vez de probar la aplicación manualmente, puedes pedir al agente que abra una página, navegue por ella, aplique filtros y te comunique el resultado. Es una forma rápida de confirmar que la funcionalidad funciona sin salir de la conversación.

Internamente, el servidor MCP de Playwright utiliza el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de capturas de pantalla. Así, el agente razona sobre elementos estructurados y etiquetados (botones, enlaces y elementos de lista), como hacen las tecnologías de asistencia. Por eso, una comprobación funcional también sirve como revisión básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe la funcionalidad de filtrado:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador mediante el servidor MCP de Playwright, seguirá los pasos y te comunicará sus observaciones. Compara el informe con los criterios de aceptación de la incidencia. Si algo no parece correcto, haz preguntas de seguimiento o pide a Copilot que corrija el código antes de abrir la PR.

> [!NOTE]
> Para esta prueba, la aplicación debe estar ejecutándose en `http://localhost:4321`. Si detuviste el servidor de desarrollo, vuelve a iniciarlo antes de enviar la solicitud. La primera vez que Copilot utilice el servidor MCP de Playwright, quizá tenga que descargar un navegador. Si indica que falta uno, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de incorporación de cambios

Ahora que has confirmado que la funcionalidad funciona, puedes abrir una solicitud de incorporación de cambios (PR) para que el equipo la revise. Primero publicarás la rama `filtering-vscode`.

1. Abre el panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Captura de pantalla del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para subir la rama al repositorio.

## Crea la solicitud de incorporación de cambios

Hay varias formas de crear una solicitud de incorporación de cambios, por ejemplo, desde github.com o con la interfaz de línea de comandos (CLI) de GitHub. Ya que estás trabajando con GitHub Copilot, pídele que cree la PR: puede encontrar la incidencia correspondiente y asociarla a la solicitud.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Confirma que **Agent** está seleccionado en el selector de agentes para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Si se te solicita, selecciona **Continue** para permitir que Copilot recopile información y realice operaciones. La primera vez que use una herramienta de GitHub, quizá debas autenticarte. Sigue las instrucciones para permitir el acceso.
5. Observa cómo Copilot busca entre las incidencias, encuentra la adecuada y crea la PR.
6. Selecciona el enlace que genera Copilot para revisar la solicitud, pero **no la combines todavía**.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio:

- Aprendiste qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot.
- Añadiste el servidor MCP de Playwright al espacio de trabajo.
- Lo usaste para probar manualmente la funcionalidad de filtrado en un navegador antes de publicarla.
- Publicaste la rama y abriste una solicitud de incorporación de cambios.

Has utilizado un servidor MCP para *probar* una funcionalidad, pero MCP es solo una de las formas de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [Registro de GitHub MCP][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
