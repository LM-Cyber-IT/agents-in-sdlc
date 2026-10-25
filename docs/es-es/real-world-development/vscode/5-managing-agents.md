---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] |
|:--|

En el modo agente, Copilot explora el código, propone cambios, edita archivos y ejecuta comandos. Copilot Chat en VS Code muestra las llamadas a herramientas y los cambios a medida que se producen. Puedes revisar las diferencias, aceptar o rechazar los cambios individuales y dirigir la conversación con prompts de seguimiento.

En este ejercicio, harás lo siguiente:

- Supervisarás en tiempo real el trabajo del agente de accesibilidad.
- Revisarás y aceptarás los cambios propuestos en los archivos.
- Pedirás al agente, durante la misma sesión, que añada un control de modo claro.

## Escenario

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño pide también un control de modo claro para los usuarios a quienes el tema oscuro les resulte demasiado oscuro. En lugar de empezar otra conversación, ampliarás la sesión actual.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada cambio incluye una diferencia con los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para revisar las llamadas a herramientas, las lecturas y modificaciones de archivos y los comandos del terminal.
3. Cuando aparezca una edición, selecciona **Keep** para aceptarla o **Undo** para revertirla y dejar que el agente vuelva a intentarlo.
4. Abre **Files changed** en la parte superior del chat para ver los archivos modificados durante la sesión.
5. Espera a que el agente termine el modo de alto contraste y lea el mensaje final con el resumen de los cambios.

> [!NOTE]
> Si una llamada a una herramienta pide permiso, por ejemplo, para ejecutar un comando, revisa la solicitud y acepta solo las operaciones que consideres adecuadas.

## Guía al agente para añadir el modo claro

Amplía la misma conversación para añadir un control de modo claro. Así, el agente conserva el contexto de los componentes y estilos que ya ha modificado y del patrón de almacenamiento local que ha usado.

1. En el cuadro de Copilot Chat, envía este prompt:

   ```plaintext
   Buen trabajo. Ahora añade un control de modo claro junto al de alto contraste. El modo claro debe seguir el mismo patrón, conservarse en el almacenamiento local y estar disponible en la misma zona de la interfaz. El usuario debe poder activar el modo claro y el de alto contraste de forma independiente.
   ```

2. Deja que el agente explore los cambios existentes y proponga los cambios del modo claro.
3. Revisa cada cambio nuevo y selecciona **Keep** o **Undo**.
4. Si el agente toma una dirección que no quieres, por ejemplo, refactoriza el control de forma inesperada, detenlo con el botón **Stop** junto al cuadro de chat y envía un prompt para aclarar lo que necesitas.

> [!TIP]
> Suele ser más rápido dirigir una sesión en curso que empezar de nuevo. Puedes pedir ajustes, como simplificar las hojas de estilo en cascada (CSS), corregir el enfoque o ampliar el alcance.

## Revisa los cambios

Antes de confirmar el trabajo, revisa todos los archivos que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa los archivos modificados, incluidos los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Confirma que el código incorpora patrones de accesibilidad, como atributos de Accessible Rich Internet Applications (ARIA), navegación con teclado, lenguaje de marcado de hipertexto (HTML) semántico y persistencia en el almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel **Source Control**. Los publicarás en el ejercicio siguiente.

## Resumen y siguientes pasos

Has usado Copilot Chat en modo agente para supervisar y guiar una sesión en curso. Has revisado los cambios, aceptado o rechazado ediciones y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En este ejercicio:

- Supervisaste el trabajo del agente en tiempo real.
- Aceptaste y rechazaste cambios individuales en los archivos.
- Guiaste una sesión en curso con prompts de seguimiento.

En el [siguiente ejercicio][next-lesson] publicarás el trabajo de accesibilidad como solicitud de incorporación de cambios y revisarás los cambios del agente personalizado.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Siguiente ejercicio: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
