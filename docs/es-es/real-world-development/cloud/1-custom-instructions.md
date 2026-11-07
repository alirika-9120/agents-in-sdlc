---
title: "Ejercicio 1: Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de Copilot en la nube →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe hacerse de una forma concreta, o si Copilot necesita información previa, asegúrate de que tenga acceso a ese contexto. Los [archivos de instrucciones][instruction-files] permiten proporcionar directrices a Copilot para que entienda no solo *qué* quieres que haga, sino también *cómo* quieres que lo haga.

En este ejercicio:

- Explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y las instrucciones específicas de rutas.
- Añadirás un estándar nuevo para todo el repositorio en `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los itinerarios de VS Code y CLI, aquí no compararás una solicitud *antes y después*. El agente de Copilot en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil mostrar el efecto en tiempo real. Verás cómo influyen las instrucciones más adelante, cuando revises las solicitudes de incorporación de cambios que genere el agente en la nube.

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

## Añade un estándar para todo el repositorio

El siguiente paso es la única edición que harás en este ejercicio: añadir una regla para todo el proyecto que indique que la documentación debe incluirse en el código mediante comentarios TSDoc y un comentario de cabecera del archivo. El agente en la nube utilizará esta directriz cuando trabaje en incidencias que le asignes más adelante.

Antes de editar el archivo, prepara una rama de trabajo. El agente en la nube leerá las instrucciones de la rama a la que apunte la incidencia, pero confirmarás los cambios por separado:

1. En el terminal del codespace, crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como se indicó antes, `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añade a los estándares de programación las directrices que faltan para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que describe los estándares de programación, pero no incluye directrices para documentar el código; por eso la función auxiliar generada no tenía comentarios de documentación.
3. Debajo de los estándares actuales, añade estas líneas de Markdown para indicar a Copilot que añada comentarios de cabecera y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste en la lección anterior, puedes crear archivos de instrucciones en el nivel del repositorio (`.github/copilot-instructions.md`) para definir directrices globales, o archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas. El archivo del repositorio es el lugar adecuado para estándares globales como la regla de comentarios de documentación que acabas de añadir.

## Confirma, publica y combina la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que apunta la incidencia. En el siguiente ejercicio asignarás una incidencia a Copilot, que creará una rama a partir de `main`. Por eso, los cambios de instrucciones deben llegar a `main` para que el agente en la nube pueda utilizarlos.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` a `main` y combínala. Así el agente en la nube podrá leer las instrucciones al trabajar en las incidencias que le asignes en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir la rama y confirmar los cambios directamente en `main`. El paso de la rama permite practicar cómo se trabajaría en un proyecto real.

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y has añadido un estándar nuevo para todo el repositorio en `.github/copilot-instructions.md`. Verás cómo se aplica en las solicitudes de incorporación de cambios que genere el agente en la nube durante el resto del itinerario.

A continuación, [asignarás tu primera incidencia al agente de Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
