---
title: "Lección 4: Guía a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, conviene poner esa orientación a disposición de Copilot. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones que ha seguido Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- Explora cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas.
- Actualiza el archivo de instrucciones para garantizar que se siguen los estándares de programación.
- Observa el efecto de los archivos de instrucciones en el código.

## Escenario

Como cualquier empresa de desarrollo, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo. Entre ellos:

- Los comentarios deben explicar la intención y las decisiones que no sean obvias, en lugar de repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar con TSDoc/JSDoc su propósito, parámetros y valores devueltos, incluido el argumento `db` inyectable cuando exista.
- Los componentes reutilizables de Astro deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Deben conservarse las directrices de formato y lint existentes.

Con los archivos de instrucciones darás a Copilot la información necesaria para que realice las tareas de acuerdo con estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo y tus requisitos de programación. Te permiten orientar sus sugerencias y fragmentos de código con las convenciones, bibliotecas y comentarios que prefieras. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo de instrucciones que se envía a Copilot con **todas** las solicitudes del repositorio. Debe contener información del proyecto que sea pertinente para la mayoría de las solicitudes.
- `.github/instructions/*.instructions.md`: archivos con directrices para lenguajes, tipos de archivo o tareas concretas.

> [!NOTE]
> La disponibilidad de otros formatos de instrucciones varía según el entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explora los archivos de instrucciones personalizadas del proyecto

El proyecto inicial ya incluye varios archivos de instrucciones. Explóralos antes de hacer cambios para observar su efecto.

1. Vuelve a tu codespace.
2. En el editor de Codespaces, no en el terminal, abre `.github/copilot-instructions.md`.
3. Revisa la descripción breve del proyecto y las directrices de programación. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora sus archivos. Hay instrucciones para archivos de Astro, la capa de datos de Drizzle, las pruebas y otros ámbitos.
5. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior: define el patrón global que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y fíjate en los enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar a Copilot ejemplos que debe seguir.

## Actualiza los archivos de instrucciones según las directrices del equipo

Los archivos actuales son un buen punto de partida, pero falta una directriz. Modifica el archivo principal `copilot-instructions.md` para garantizar que el código TypeScript generado incluya comentarios TSDoc.

1. En `.github/copilot-instructions.md`, busca la sección **Code formatting guidance**, que debería estar alrededor de la línea 35.
2. Añade el siguiente elemento al final de la lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

¡El archivo se guardará automáticamente!

## Aplica las directrices actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar las instrucciones, sal de la conversación de filtrado y reanúdala para que Copilot cargue las nuevas directrices sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio y en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.
3. Pide a Copilot que aplique las directrices actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Observa los nuevos comentarios TSDoc y confirma que describen el código con precisión.

## Resumen y siguientes pasos

Has explorado cómo Copilot CLI incorpora el contexto de los archivos de instrucciones y has aplicado un nuevo estándar a la funcionalidad. En concreto:

- Exploraste cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas.
- Actualizaste el archivo de instrucciones para garantizar que se siguen los estándares de programación.
- Observaste el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable quality-checks][next-lesson] para mantener la coherencia de las pruebas y el lint.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
