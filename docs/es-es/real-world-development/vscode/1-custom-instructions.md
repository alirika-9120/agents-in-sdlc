---
title: "Ejercicio 1: Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe hacerse de una forma concreta, o si Copilot necesita información previa, asegúrate de que tenga acceso a ese contexto. Hay varias formas de proporcionar contexto específico a Copilot. Entre las principales están los [archivos de instrucciones][instruction-files], que permiten orientar la generación de código.

En este ejercicio:

- Explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y las instrucciones específicas de rutas.
- Generarás la primera parte de los datos para el filtrado (una función auxiliar para los editores) con las instrucciones actuales.
- Añadirás un estándar nuevo para todo el repositorio en `.github/copilot-instructions.md`.
- Volverás a enviar la misma solicitud y observarás cómo el código generado sigue el nuevo estándar.
- Confirmarás el cambio de instrucciones y la parte de filtrado en `main` para que Copilot pueda utilizar las nuevas directrices en el siguiente ejercicio.

> [!CAUTION]
> Es posible que el código generado no cumpla algunos de los estándares que hayas definido. Copilot no es determinista. El objetivo del ejercicio es observar la tendencia tras actualizar las instrucciones, no obtener exactamente el mismo resultado carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo. Entre ellos:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su función.

Con los archivos de instrucciones te asegurarás de que Copilot dispone de la información necesaria para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo y tus requisitos de programación. Te permiten orientar sus sugerencias con las convenciones, bibliotecas y comentarios que prefieras. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo de instrucciones que se envía a Copilot con **todas** las solicitudes del repositorio. Debe contener información del proyecto pertinente para la mayoría de las solicitudes que se envían a Copilot Chat o Copilot CLI, como la pila tecnológica, una descripción general del proyecto, las prácticas recomendadas y otras directrices globales.
- `.github/instructions/*.instructions.md`: archivos que puedes crear para tareas o tipos de archivo concretos. Permiten definir directrices para lenguajes como TypeScript o Astro, o para tareas como crear un componente de interfaz de usuario o un conjunto de pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código con Copilot Chat, no para completar código ni para las sugerencias del siguiente cambio.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo`.
>
> Copilot también [admite otros formatos de archivos de instrucciones][custom-instructions-support], incluidos AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Desarrollar en detalle todos los aspectos de la creación de archivos de instrucciones queda fuera del alcance de este taller. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. En general:

- Mantén `copilot-instructions.md` centrado en directrices generales del proyecto, como la descripción de lo que se está desarrollando, la estructura del proyecto y los estándares globales de programación.
- Usa `*.instructions.md` para instrucciones específicas de tipos de archivo (pruebas unitarias, componentes de Astro o capa de datos) o de tareas concretas.
- Escribe en lenguaje natural. Expón las directrices con claridad e incluye ejemplos del aspecto que debe y no debe tener el código.

No hay una única forma correcta de crear archivos de instrucciones, igual que no hay una única manera de usar la IA. La experimentación te ayudará a averiguar qué funciona mejor en tu proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería disponer de un conjunto sólido de archivos de instrucciones. Mientras exploras los archivos de este proyecto, quizá encuentres algunos para tareas como [actualizar la interfaz de usuario][ui-instructions] o trabajar con [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El ejercicio de cada entorno indicará cuándo utilizarla.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas de este proyecto

Dedica un momento a leer los archivos de instrucciones que incluye este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la descripción breve del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas sobre **GitHub Actions Workflows**. Estas directrices se aplican a cualquier interacción con Copilot en este repositorio.
3. Abre la carpeta `.github/instructions` y explora sus archivos. Hay instrucciones para archivos de Astro, la capa de datos de Drizzle, las pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior: define un patrón global relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones. En este caso, coincide con los archivos de pruebas TypeScript, por ejemplo, los que siguen el patrón `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar a Copilot ejemplos que debe seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación del proyecto, pero aún no exige documentar el código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los comentarios de cabecera de los archivos.

## Crea una rama para los cambios

Sigue las prácticas recomendadas y crea una rama para estos cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. La selección de modo y modelo varía según el ejercicio; cada paso indicará las opciones necesarias.

Para observar el efecto de las instrucciones personalizadas, empieza enviando una solicitud con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar la misma solicitud para comparar los resultados.

1. Cierra las pestañas abiertas del editor de los ejercicios anteriores para que Copilot solo utilice el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse la función auxiliar.
3. Selecciona **Agent** en el selector de agentes de la vista Chat para que Copilot pueda aplicar cambios a los archivos.

   ![Captura de pantalla que muestra el selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía esta solicitud:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios al código, a menudo tanto al archivo de la función auxiliar como a sus pruebas.
6. Observa que la función auxiliar propuesta está tipada, recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editores. Esto se debe a las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y el comentario de cabecera del archivo.

> [!CAUTION]
> Como Copilot no es determinista, puede que añada comentarios de documentación aunque no se lo pidas. No pasa nada; el objetivo es observar la mejora de coherencia tras actualizar las instrucciones.

## Añade un estándar para todo el repositorio

Como se indicó antes, `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añade a los estándares de programación las directrices de documentación que faltan.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que documenta los estándares de programación, pero no incluye directrices para documentar el código. Por eso la función auxiliar generada no tenía comentarios de documentación.
3. Debajo de los estándares actuales, añade estas líneas de Markdown para indicar a Copilot que añada comentarios de cabecera y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste en la lección anterior, puedes crear archivos de instrucciones en el nivel del repositorio (`.github/copilot-instructions.md`) para definir directrices globales, o archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas. El archivo del repositorio es el lugar adecuado para estándares globales como la regla de comentarios de documentación que acabas de añadir.

## Vuelve a enviar la solicitud y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para limpiar el contexto.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía **la misma solicitud** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que ahora el archivo propuesto empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Funciones auxiliares de acceso a datos de editores para la plataforma de financiación colectiva de Tailspin Toys.
    * Proporciona funciones para recuperar información de los editores de la base de datos.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todos los editores con su id y nombre.
    *
    * @param db - El cliente de base de datos de Drizzle.
    * @returns Una promesa que se resuelve en una matriz de objetos de editor.
    */
   ```

Has orientado a Copilot para que siga un estándar nuevo del proyecto y lo aplique al código real que utilizará el siguiente ejercicio.

## Confirma las instrucciones y publica la rama

Los archivos de instrucciones son recursos del repositorio y se gestionan con el mismo sistema de control de versiones que los demás archivos. Confirma los cambios y sube la rama al repositorio.

1. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. En el terminal, confirma que están presentes las actualizaciones de instrucciones y de la función auxiliar:

   ```bash
   git status
   ```

3. Prepara y confirma las actualizaciones de instrucciones y todos los archivos que Copilot haya modificado para crear la función auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados (por ejemplo, en las pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Sube la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Ya has publicado la rama. Ahora crearás una solicitud de incorporación de cambios y la asociarás a la incidencia del backlog relacionada con las instrucciones de documentación. Copilot puede buscar la incidencia, crear la PR y combinarla.

> [!NOTE]
> En este ejercicio Copilot realizará acciones en GitHub por primera vez. La plantilla del proyecto ya conecta las herramientas de GitHub con tu espacio de trabajo, así que no tienes que configurar nada. La primera vez que Copilot utilice una herramienta de GitHub, es posible que VS Code te pida que inicies sesión. Sigue las instrucciones para permitirlo. En un ejercicio posterior aprenderás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat en tu codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que busque la incidencia relacionada con la actualización de los archivos de instrucciones y cree una PR desde la rama actual. Indícale que describa tanto las actualizaciones de instrucciones como la base de la función auxiliar de editores:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot empezará a buscar la incidencia y a crear la PR.
5. Si Copilot te pide **Allow** para realizar acciones en GitHub, revisa el comando y selecciona **Allow** cuando corresponda.
6. Cuando se cree la PR, pide a Copilot que la combine y vuelva a `main` en tu entorno local:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Si Copilot te pide **Allow** para realizar acciones en GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** cuando corresponda.

¡Has creado y combinado una solicitud de incorporación de cambios con ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- Enviar una solicitud de generación de código y observar el resultado con las instrucciones *existentes*.
- Añadir un estándar para todo el repositorio en `.github/copilot-instructions.md`.
- Volver a enviar la misma solicitud y observar cómo el código propuesto sigue el nuevo estándar.
- Confirmar las actualizaciones de instrucciones y la base de la función auxiliar en `main` para que el siguiente ejercicio pueda aprovecharlas.

A continuación, pondrás en práctica estas instrucciones en el [modo agente][next-lesson], mientras Copilot añade una funcionalidad al código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
