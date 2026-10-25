---
title: "Lección 2 - Añadir valoraciones por estrellas: una mejora rápida"
description: "Inicia tu primera sesión de agente en la aplicación GitHub Copilot, realiza un pequeño cambio en las tarjetas de juegos y combínalo como tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Ya has explorado el espacio de trabajo y usado un chat rápido. Ahora iniciarás una **sesión de agente** y harás tu primer cambio en el proyecto. Será pequeño: los datos de los juegos ya incluyen una valoración por estrellas, pero las tarjetas de la página principal no la muestran. Pedirás al agente que la muestre, revisarás el cambio y lo combinarás como tu primera solicitud de incorporación de cambios.

En esta lección, harás lo siguiente:

- Iniciarás una sesión de agente y aprenderás cómo se estructura.
- Pedirás al agente que haga un cambio pequeño y específico en el proyecto.
- Revisarás el cambio en la vista de diferencias del espacio de trabajo.
- Ejecutarás la aplicación en local y comprobarás el cambio en el navegador.
- Abrirás y combinarás tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración por estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas de la página principal solo muestran el título, la categoría, la editorial y la descripción. Para empezar, pedirás al agente que muestre la valoración existente en cada tarjeta: un cambio pequeño y aislado, ideal para tu primera sesión.

## Estructura de una sesión

Una **sesión** es una conversación con un agente que se ejecuta en su propio espacio de trabajo aislado. Cada sesión tiene un **árbol de trabajo y una rama de Git dedicados**, lo que permite ejecutar varias sesiones a la vez sin que sus cambios entren en conflicto. Las sesiones aparecen en la barra lateral, agrupadas por repositorio; selecciona una para cambiar a ella.

En una sesión verás la conversación con el agente, la actividad de sus herramientas mientras explora y edita archivos, y la lista de archivos modificados con sus diferencias.

## Inicia una sesión y solicita el cambio

Durante la configuración de la aplicación, añadiste el proyecto desde su repositorio de GitHub. Inicia una sesión nueva para ese repositorio y pide el cambio.

1. Vuelve a la aplicación GitHub Copilot o ábrela.
2. Selecciona **+** en la parte superior de la barra lateral para iniciar una sesión.
3. Selecciona `tailspin-toys` como repositorio.
4. Debajo del cuadro del prompt, selecciona un **new working tree** y el modo **Interactive**. Envía este prompt:

   ```plaintext
   Muestra en las tarjetas de la lista la valoración starRating de cada juego sobre 5. Si la valoración es null, muestra "No rating yet". Mantén el diseño de las tarjetas, añade pruebas y ejecuta las comprobaciones pertinentes.
   ```

5. Pulsa <kbd>Enter</kbd> para enviar el prompt.

La aplicación creará un árbol de trabajo aislado, explorará el proyecto y localizará los archivos que debe actualizar para añadir la funcionalidad.

## Revisa la diferencia

Revisa todos los cambios generados por inteligencia artificial (IA) antes de combinarlos, incluso los pequeños.

1. En la esquina superior derecha, selecciona **Toggle review panel**. En el panel que se abre, selecciona **Changes** para ver los archivos modificados y sus diferencias.

   ![Barra superior de la aplicación GitHub Copilot con una flecha que señala el botón Toggle review panel situado a la derecha de Create PR](../../../_images/app-2-review-panel.png)

2. Revisa el código añadido a `GameCard.astro`. Debería mostrar la valoración cuando exista y usar `No rating yet` cuando `starRating` sea `null`:

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot es probabilístico y no determinista; el código exacto puede variar. Comprueba que el comportamiento es equivalente al ejemplo.

## Comprueba los cambios

Antes de abrir un navegador, revisa los resultados de las comprobaciones automatizadas. Confirma que las pruebas cubren un `starRating` numérico y la alternativa para `null`. Si falta un requisito previo o se ha omitido una comprobación, no la des por superada y revisa cualquier solicitud de instalación antes de aprobarla.

Pide a Copilot que abra el sitio en un lienzo de navegador integrado en la aplicación.

> [!TIP]
> Un lienzo es un widget interactivo dentro de la aplicación Copilot. Más adelante explorarás lienzos personalizados; por ahora usarás el navegador integrado.

1. Envía este prompt para que Copilot inicie la aplicación y la abra en el lienzo del navegador:

   ```plaintext
   Inicia la aplicación y ábrela en el lienzo del navegador.
   ```

2. Cuando se inicie la aplicación, espera a que se abra el navegador dentro de la aplicación Copilot.
3. Confirma que las tarjetas de juegos con valoración muestran su puntuación sobre cinco.
4. Cuando termines, pide a Copilot que detenga el servidor de desarrollo y cierre el lienzo:

   ```plaintext
   Detén el servidor de desarrollo y cierra el lienzo del navegador.
   ```

## Abre y combina tu primera solicitud de incorporación de cambios

Ahora crearás una solicitud de incorporación de cambios (PR) para incorporar el código a la base existente.

1. Selecciona **Create PR** en la esquina superior derecha.
2. Si se te solicita, selecciona **Sign in with your browser** y sigue las instrucciones para autenticarte.
3. Si Copilot explica que la política del repositorio exige una incidencia relacionada para crear la PR, confirma que debe seguir ese proceso.
4. Deja que Copilot cree la PR.
5. Selecciona la burbuja **PR** encima del chat para abrir la solicitud en el panel de revisión.
6. Cuando esté lista, selecciona **Ready to merge**.
7. En el nuevo cuadro de diálogo, selecciona **Merge pull request** para combinarla.

## Resumen y siguientes pasos

Has entregado tu primer cambio con la aplicación GitHub Copilot. En concreto:

- Iniciaste una sesión de agente y aprendiste cómo se estructura.
- Pediste al agente que hiciera un cambio pequeño y específico en las tarjetas de juegos.
- Revisaste el cambio en la vista de diferencias del espacio de trabajo.
- Ejecutaste la aplicación en local y comprobaste la valoración por estrellas en el navegador.
- Abriste la PR 1, revisaste sus comprobaciones y la combinaste explícitamente.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para desarrollar una funcionalidad más amplia.

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#instala-y-configura-la-aplicacion-github-copilot
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
