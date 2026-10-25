---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los ejercicios anteriores asignaste a Copilot en la nube tres tareas relacionadas con la experiencia de usuario y nuevas funcionalidades. Aunque el agente trabaja de forma asíncrona y autónoma, es importante supervisar esas tareas.

La [página de agentes][agents-page] de GitHub.com permite ver las tareas con solicitudes de incorporación de cambios (PR) abiertas, consultar las operaciones realizadas y orientar una sesión en curso.

En este ejercicio, harás lo siguiente:

- Explorarás la página de agentes para supervisar las tareas del agente en la nube.
- Guiarás una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de pedir un modo de alto contraste, el equipo decidió añadir también un modo claro. Como el agente ya estaba trabajando en los estilos del sitio y en los controles de alternancia, le pedirás que amplíe el trabajo.

## Revisa las tareas del agente de Copilot en la nube

Consulta el estado de las tareas asignadas al agente.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Revisa las tareas del panel principal y del panel izquierdo. Deberías ver las tareas asignadas, como documentar el código, generar interfaces de programación de aplicaciones (API) para modificar juegos y añadir un modo de alto contraste.
3. Selecciona una tarea en curso y revisa las operaciones realizadas, que pueden incluir clonar el código, preparar el entorno, configurar servidores de Model Context Protocol (MCP) y completar el trabajo.

> [!NOTE]
> Los pasos concretos que aparecen dependen del estado de la tarea y del enfoque del agente.

4. Revisa también el panel de la PR a la derecha, donde puedes ver la solicitud y los archivos modificados.

## Guía al agente en la nube

Pide a Copilot que añada un control para el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión que corresponda al modo de alto contraste. El título exacto depende del nombre que haya elegido Copilot y del estado de la tarea.

   ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión hasta que indique que ha terminado la configuración y ha empezado a trabajar.
3. En el cuadro **Steer active session while Copilot is working**, escribe:

   ```plaintext
   Mientras trabajamos en el modo de alto contraste, añadamos también un modo claro. Debe haber un control para este modo, para que los usuarios puedan seleccionar cómo quieren ver el sitio.
   ```

   ![Tarea del agente en la nube con el cuadro para guiar la sesión activa](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Comprueba que Copilot confirma el prompt y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot incorporará la nueva solicitud al flujo cuando termine el paso que estaba realizando al recibirla. La tarea tardará unos minutos; mientras tanto, puedes repasar lo aprendido.

## Resumen y siguientes pasos

Has explorado la página de agentes de Copilot, el espacio central para supervisar y orientar las tareas del agente en la nube. Permite seguir las tareas activas y completadas, revisar el trabajo y redirigir una sesión en curso. En este ejercicio:

- Exploraste el centro de control y la página de agentes para supervisar tareas.
- Guiaste una sesión en curso para pedir una funcionalidad adicional.

Cuando Copilot termine las funciones de accesibilidad, [iterarás sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Siguiente ejercicio: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
