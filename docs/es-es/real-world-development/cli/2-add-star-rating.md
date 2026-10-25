---
title: "Lección 2 - Añadir valoraciones por estrellas: una mejora rápida"
description: "Usa Copilot CLI para hacer un pequeño cambio en las tarjetas de juegos, revisarlo en un navegador a través de un puerto reenviado de Codespaces y combinarlo como tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has instalado Copilot CLI y probado una conversación. Ahora harás tu primer cambio en el proyecto, procurando que sea pequeño: los datos de los juegos ya incluyen una valoración por estrellas, pero las tarjetas de la página principal no la muestran. Pedirás al agente que la muestre, revisarás el cambio y lo combinarás como tu primera solicitud de incorporación de cambios.

En esta lección, harás lo siguiente:

- Iniciarás una conversación de Copilot centrada en una rama de funcionalidad.
- Pedirás al agente que haga un pequeño cambio en el proyecto.
- Revisarás el cambio con `/diff`.
- Ejecutarás la aplicación y comprobarás el cambio en un navegador a través de un puerto reenviado de Codespaces.
- Abrirás y combinarás tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración por estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas de la página principal solo muestran el título, la categoría, la editorial y la descripción. Para empezar, pedirás al agente que muestre en cada tarjeta la valoración existente: un cambio pequeño y aislado, ideal para tu primera sesión.

## Estructura de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de la CLI utiliza el repositorio y la rama que tengas en el terminal, en lugar de crear un árbol de trabajo independiente. Las conversaciones guardadas permiten volver al mismo intercambio; los archivos y la rama siguen siendo el estado normal de Git en el disco.

En una conversación verás los prompts y las respuestas del agente, la actividad de las herramientas mientras explora y edita archivos, y los cambios que puedes inspeccionar con `/diff`.

## Inicia una conversación y pide el cambio

Inicia una conversación nueva para empezar a implementar la funcionalidad.

1. Vuelve a tu Codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si Copilot no está iniciado, ejecuta este comando:

   ```bash
   copilot --yolo
   ```

4. Inicia una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Usa este prompt para pedir el cambio:

   ```plaintext
   Muestra en las tarjetas de la lista la valoración starRating de cada juego sobre 5. Si la valoración es null, muestra "No rating yet". Mantén el diseño de las tarjetas, añade pruebas y ejecuta las comprobaciones pertinentes.
   ```

Copilot explorará el proyecto, localizará los archivos que muestran los detalles de los juegos y creará el código necesario. Ya has añadido una funcionalidad con Copilot CLI.

## Revisa la diferencia

Todos los cambios generados por inteligencia artificial (IA) deben revisarse antes de combinarlos, incluso los pequeños. Revisa los cambios directamente en Copilot CLI.

1. Escribe `/diff` e inspecciona todos los archivos modificados.
2. Confirma que la tarjeta muestra la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos estados.
4. Revisa los resultados de las comprobaciones que ejecutó Copilot y pídele que corrija los fallos.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la pantalla de diferencias.

> [!NOTE]
> Copilot, como todas las herramientas de IA generativa, es probabilístico y no determinista, por lo que el código exacto puede variar. Revisa el comportamiento en lugar de esperar una implementación concreta.

## Comprueba los cambios

No basta con leer el código y suponer que funciona. Pide a Copilot que inicie el sitio para poder examinar la interfaz actualizada en el navegador a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda comprobar en el navegador el cambio de la valoración por estrellas. Dime la dirección y deja el servidor en ejecución.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas de juegos muestran sus valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que ha iniciado:

   ```plaintext
   Detén el servidor de desarrollo que has iniciado.
   ```

## Abre y combina tu primera solicitud de incorporación de cambios

Ya has creado la funcionalidad. Ahora revisarás y combinarás el código en el proyecto mediante una solicitud de incorporación de cambios (PR).

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Confirma los cambios revisados de las valoraciones por estrellas con un mensaje de confirmación adecuado.
   ```

2. Escribe `/pr create`. Copilot CLI enviará la confirmación existente al crear la PR y mostrará su URL.
3. Abre la PR manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la combinación.
6. Vuelve a tu Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu rama local `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y siguientes pasos

¡Enhorabuena! Has entregado tu primer cambio con GitHub Copilot CLI. En concreto:

- Iniciaste una conversación centrada en una rama de funcionalidad.
- Pediste al agente que hiciera un pequeño cambio en las tarjetas de juegos.
- Revisaste el cambio con `/diff`.
- Ejecutaste la aplicación y comprobaste la valoración por estrellas en un navegador a través de un puerto reenviado de Codespaces.
- Abriste y combinaste tu primera solicitud de incorporación de cambios.

Ahora partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot para [desarrollar una funcionalidad más amplia][next-lesson].

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
