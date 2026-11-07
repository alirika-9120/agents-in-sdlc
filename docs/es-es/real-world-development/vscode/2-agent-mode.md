---
title: "Ejercicio 2: Añadir funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen afectar a varios archivos y requieren tareas como ejecutar pruebas. El flujo de trabajo de un desarrollador suele incluir localizar todos los archivos necesarios, hacer los cambios, ejecutar pruebas, depurar, averiguar qué archivo se ha pasado por alto, hacer otra actualización... y así sucesivamente.

El modo agente de Copilot te ayuda con estas tareas.

El modo agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza las actualizaciones necesarias, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo usarlo para añadir una funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente realiza las tareas en el IDE, con ciclos de respuesta rápidos e interacción directa. El agente en la nube actúa como un compañero de programación y trabaja de forma asíncrona, como un miembro del equipo, mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- El modo agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y coordinar cambios.
- El modo agente de GitHub Copilot puede implementar funcionalidades en la interfaz de usuario y la capa de datos.
- Revisar los cambios y las pruebas generados por el modo agente de Copilot antes de combinarlos con el código base.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editor. En el ejercicio anterior ya añadiste una función auxiliar para los editores. Ahora terminarás el trabajo restante en la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y consultar sus detalles. Es una única aplicación Astro que genera las páginas como HTML estático durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde el frontmatter, mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz en el cliente. Las funciones reutilizables para acceder a los datos se encuentran en `src/lib/`, y la interactividad se implementa con pequeños elementos `<script>` de Astro que usan las API estándar del DOM.

### Inicia el sitio web

Para facilitar la ejecución del sitio web, se ha configurado la tarea `npm run dev`, que inicia un único servidor de desarrollo de Astro. Sigue estos pasos para ejecutarla en GitHub Codespaces:

1. Vuelve a tu codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal nuevo en el codespace pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama nueva para el trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta este comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, verás un mensaje con la URL, similar a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pregunta si quieres abrir una ventana del navegador para `http://localhost:4321`, ciérralo seleccionando la **x**.

5. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar un enlace `localhost` en el terminal, se te redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, que ahora aloja el servidor web.

### Explora el sitio web

Cuando se esté ejecutando el sitio, explora sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, el editor y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección de modo y modelo varía según el ejercicio; cada paso indicará las opciones necesarias.

La primera versión del sitio ya funciona, pero queremos añadir más capacidades. Empecemos por revisar el backlog. Al configurar el proyecto a partir de la plantilla, se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te muestre esas incidencias.

1. Selecciona **Agent** en el selector de agentes de la vista Chat. El agente puede planificar e implementar cambios en varios archivos, ejecutar comandos de terminal y utilizar herramientas de forma autónoma.

   ![Captura de pantalla que muestra el selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no recomiendan un modelo concreto. Durante su desarrollo se utilizó Claude Sonnet 4.5, por lo que aparece en las instrucciones para intentar que las sugerencias de código sean relativamente uniformes. Sin embargo, como los modelos de lenguaje grandes (LLM) son probabilísticos, es posible que las sugerencias difieran de las que se muestran en el taller. Es normal y esperable.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot puede usar un comando MCP distinto y aun así completar la tarea.

3. Envía esta solicitud a Copilot para que muestre las incidencias del backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado los elementos según cuáles considera más útiles para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que dará contexto a Copilot. Usarás el archivo de instrucciones de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio web.

1. En tu codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices generales para añadir funcionalidad. Incluyen:
   - Una descripción general de la arquitectura.
   - Principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - Enlaces a instrucciones específicas para distintos tipos de archivo, como:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> Los archivos de instrucciones permiten enlazar a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así puedes reutilizar instrucciones, dividir conjuntos extensos en archivos más manejables e incluir ejemplos y plantillas.

## Implementa la funcionalidad de filtrado

Para completar el filtrado, tendrás que realizar al menos tres actualizaciones distintas:

- Añadir o mejorar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- Añadir o actualizar pruebas para el comportamiento del filtrado.
- Actualizar la página de listado de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse correctamente antes de combinar los cambios con el código base. El modo agente de Copilot puede encargarse de estas tareas. Añade la funcionalidad:

1. Continúa la conversación actual con Copilot o selecciona **New Chat** para empezar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de pantalla de un ejemplo en el que se selecciona el archivo de instrucciones de la interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el selector de agentes de la vista Chat.

   ![Captura de pantalla que muestra el selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que sigue seleccionado el modelo **Claude Sonnet 4.5**.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad. Debería encontrar las funciones auxiliares de la capa de datos, la interfaz y las pruebas. Después, empezará a modificar archivos y a ejecutar las pruebas.

   ![Captura de pantalla de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código base, puede tardar unos minutos. Durante el proceso quizá aparezcan los botones **Keep** y **Undo** en el editor. Cuando termine, aparecerá uno de estos botones para cada cambio; no hace falta que los selecciones mientras Copilot está trabajando.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla de un cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de pantalla de una sesión completa con el modo agente de Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y busca posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código que genera Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a usar el modo agente de GitHub Copilot para añadir capacidades al sitio web de Tailspin Toys. Has visto cómo:

- El modo agente de GitHub Copilot puede implementar funcionalidades en la interfaz de usuario y la capa de datos.
- El modo agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y coordinar cambios.
- Revisar los cambios y las pruebas generados por el modo agente de Copilot antes de combinarlos con el código base.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Aplica lo aprendido en este ejercicio y pide a Copilot que implemente la paginación. Ten en cuenta lo siguiente:

- Sigue las prácticas recomendadas y usa los archivos de instrucciones existentes.
- Decide cómo quieres implementar la paginación, por ejemplo, si el usuario podrá seleccionar el tamaño de página o si será fijo.
- En la solicitud, proporciona a Copilot las directrices necesarias para crear la implementación que quieres.
- Es posible que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Este es el flujo habitual al trabajar con Copilot.

## Recursos

- [Copilot: modos ask, edit y agent; qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
