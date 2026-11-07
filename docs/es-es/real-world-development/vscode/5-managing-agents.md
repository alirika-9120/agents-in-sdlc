---
title: "Ejercicio 5: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando usas GitHub Copilot en modo agente, trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tu equipo en tiempo real, Copilot Chat en VS Code muestra cada llamada a una herramienta y cada edición de archivo a medida que suceden. Puedes revisar cada diferencia, aceptar o rechazar cambios concretos y orientar la conversación con solicitudes de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- Supervisarás en tiempo real el trabajo del agente de accesibilidad.
- Revisarás y aceptarás los cambios propuestos en los archivos.
- Orientarás al agente durante la sesión para que añada un selector de tema claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita además un selector de tema claro para los usuarios a quienes les resulta demasiado oscuro el tema predeterminado. En vez de empezar otra conversación, orientarás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con una diferencia y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a una herramienta. Verás lecturas y ediciones de archivos y los comandos de terminal que solicita ejecutar.
3. Cuando el agente proponga una edición, aparecerá la diferencia en el chat:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertir el cambio y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados por el agente durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta solicita permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba únicamente los comandos que consideres seguros.

5. Espera a que el agente termine de implementar el modo de alto contraste. Al final debería mostrar un resumen de los cambios.

## Orienta al agente para añadir el tema claro

Ahora que ya está implementado el modo de alto contraste, ampliarás la misma conversación para añadir un selector de tema claro. Como continuarás en el mismo chat, el agente conservará todo el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilos y el uso de almacenamiento local.

1. Envía esta solicitud de seguimiento en Copilot Chat:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. El agente continuará desde donde lo dejó, explorará los cambios aplicados y propondrá los necesarios para el tema claro.
3. Revisa cada cambio nuevo cuando aparezca, como hiciste con el modo de alto contraste. Usa **Keep** o **Undo** para cada diferencia.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, si intenta reorganizar la interfaz de los selectores de una forma que no te gusta— puedes detenerlo con el botón **Stop** situado junto al campo de chat y enviar una solicitud de seguimiento que aclare el enfoque.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Envía solicitudes de seguimiento para pedir ajustes («simplifica el CSS y usa variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer selector; integra el tema claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para los dos selectores»).

## Revisa los cambios

Antes de confirmar el trabajo, revisa rápidamente todos los archivos modificados por el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos modificados y revisa las diferencias. Confirma que se aplican los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y almacenamiento de preferencias local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición de archivo, aceptado o rechazado cambios y enviado solicitudes de seguimiento para ampliar el trabajo sin perder contexto.

En esta lección:

- Supervisaste el trabajo del agente en tiempo real.
- Aceptaste y rechazaste ediciones concretas de archivos.
- Orientaste una sesión en curso con solicitudes de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios que ha generado el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
