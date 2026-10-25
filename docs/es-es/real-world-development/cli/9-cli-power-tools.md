---
title: "Lección 9 - Funciones avanzadas de GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar trabajo opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como otras interfaces de línea de comandos, GitHub Copilot CLI incluye comandos de barra para interactuar con la herramienta. Estos comandos muestran funciones avanzadas, información interna y opciones de configuración adicionales. Ya has usado `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Ahora explorarás otros comandos útiles.

En esta lección, harás lo siguiente:

- Usarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- Usarás `/model` para explorar los modelos disponibles.
- Aprenderás a usar `/share` para exportar o compartir una sesión.
- Explorarás comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

## Escenario

Ya has completado el flujo principal de la CLI. Ahora examinarás otras capacidades: gestionar el contexto, cambiar de modelo, compartir sesiones y delegar tareas opcionalmente al [agente de Copilot en la nube][about-cloud-agent].

## Explora el contexto de Copilot CLI

En tareas extensas o complejas puedes acercarte al límite de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando hace falta; también puedes inspeccionar o compactar el contexto con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y la distribución del contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Si la conversación ya era breve, quizá no haya una diferencia grande.

> [!NOTE]
> Copilot CLI compacta el contexto automáticamente cuando se llena la ventana. Usa `/compact` para elegir el momento. Usa `/clear` o `/new` si empiezas una tarea distinta y quieres una conversación nueva.

## Elige un modelo

Los modelos tienen distintas capacidades y cada desarrollador puede tener sus preferencias. Copilot CLI permite ver los modelos disponibles y seleccionar uno.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información de uso.
3. Conserva el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Compartir el trabajo y lo aprendido ayuda al equipo a mejorar el uso de herramientas de inteligencia artificial (IA). El comando `/share` puede exportar una sesión como archivo Markdown o como archivo de lenguaje de marcado de hipertexto (HTML), crear un enlace para compartirla o publicar un GitHub gist.

1. Escribe `/help` y revisa las opciones de `/share` de la versión instalada.
2. Si quieres compartir la sesión, elige el destino adecuado; por ejemplo, `/share file` para exportarla localmente como Markdown.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o gist es opcional. No publiques contenido del repositorio o la conversación que el equipo no quiera compartir.

## Opcional: ampliar o delegar el trabajo

El flujo principal del taller ya ha terminado. Copilot CLI también incluye comandos para tareas más amplias:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo Git aislado para otra tarea.
- `/delegate` puede enviar una tarea al agente de GitHub Copilot en la nube, que trabaja de forma asíncrona y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales, ya que pueden generar trabajo adicional en árboles de trabajo o a distancia. Antes de probarlos, empieza una tarea nueva y bien delimitada y revisa los resultados con tu flujo habitual. Si quieres profundizar en el trabajo asíncrono con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI te permiten configurarlo, compartir sesiones y consultar lo que ocurre. En esta lección:

- Usaste `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- Usaste `/model` para explorar los modelos disponibles.
- Aprendiste a usar `/share` para exportar o compartir una sesión.
- Exploraste comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

Hay más comandos de barra y funciones de Copilot CLI que puedes explorar. Para terminar, [repasarás lo aprendido y los siguientes pasos][next-lesson].

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente de Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
