---
title: "Ejercicio 2 - Añadir funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios sencillos suelen afectar a varios archivos y requieren ejecutar pruebas. Hay que localizar los archivos, hacer cambios, ejecutar pruebas, depurar y corregir posibles omisiones. El modo agente de Copilot puede explorar el proyecto, coordinar cambios entre archivos, ejecutar tareas y corregir errores detectados. En este ejercicio lo usarás para añadir una funcionalidad.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE, con ciclos de respuesta rápidos e interacción directa. El agente en la nube trabaja de forma asíncrona en GitHub mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo el modo agente explora el proyecto, implementa cambios en la interfaz y la capa de datos, y genera cambios y pruebas que debes revisar antes de combinarlos.

## Escenario

El catálogo de juegos crece y quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior creaste una función auxiliar para las editoriales; ahora completarás con el modo agente el trabajo pendiente en la capa de datos, la interfaz y las pruebas.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio para conocer su funcionalidad actual. Tailspin Toys es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Es una aplicación Astro que genera páginas en lenguaje de marcado de hipertexto (HTML) estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle, un mapeador objeto-relacional (ORM); no hay una interfaz de programación de aplicaciones (API) de backend independiente ni un framework de interfaz del lado del cliente. Las funciones auxiliares de acceso a datos se encuentran en `src/lib/` y la interactividad se implementa con scripts Astro acotados y las API estándar del modelo de objetos del documento (DOM).

### Inicia el sitio web

La tarea `npm run dev` inicia el servidor de desarrollo de Astro.

1. Vuelve a tu Codespace; después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama nueva para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

5. Cuando el servidor esté listo, abre la dirección `http://localhost:4321` que aparece en el terminal. Puedes mantener pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionar el enlace.

> [!NOTE]
> En Codespaces, el enlace `localhost` del terminal redirige a `https://<your-codespace-name>-4321.app.github.dev/`, un túnel privado al servidor del Codespace.
>
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321`, ciérralo con **x**.

> [!NOTE]
> También puedes ejecutar el taller en un [contenedor de desarrollo][dev-containers] local. Abre el repositorio clonado en VS Code y selecciona **Reopen in Container**.

### Explora el sitio web

Con el sitio en ejecución, explora la lista de juegos y sus páginas de detalles. La página principal muestra títulos, imágenes y descripciones; al seleccionar un juego, verás más detalles, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al Codespace, abre el panel Copilot Chat y selecciona **New Chat**. El modo y el modelo varían según el ejercicio; cada paso los indicará cuando corresponda.

El proyecto ya incluye un backlog de incidencias. Pide a Copilot que te las muestre.

1. En el selector de agentes de la vista Chat, selecciona **Agent**.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos.
3. Envía este prompt:

   ```plaintext
   Muéstrame el backlog de elementos de mi repositorio de GitHub. Ayúdame a priorizarlos según su utilidad para los usuarios.
   ```

4. Selecciona **Continue** para permitir que Copilot enumere las incidencias.
5. Revisa la lista que genera Copilot y su priorización.

> [!CAUTION]
> Los autores del taller no recomiendan un modelo concreto. Usaron Claude Sonnet 4.5 para obtener sugerencias relativamente coherentes. Como los modelos de lenguaje de gran tamaño (LLM) son probabilísticos, es normal que las sugerencias varíen.

> [!NOTE]
> Copilot puede usar un comando distinto del Model Context Protocol (MCP) y completar la tarea correctamente.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones de interfaz de usuario (UI), que ofrece contexto para añadir funcionalidades.

1. En el Codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa la arquitectura, las prácticas de diseño de componentes, la capacidad de prueba y la accesibilidad, y los enlaces a instrucciones para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar otros archivos de instrucciones y archivos del proyecto. Las rutas son relativas al archivo de instrucciones.

## Implementa el filtrado

El filtrado requiere completar la lógica de la capa de datos (`src/lib/`), las pruebas y la interfaz de la lista de juegos. Pide al modo agente que coordine estos cambios y ejecute las comprobaciones.

1. Continúa la conversación actual o selecciona **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui**.

   ![Selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que sigue seleccionado **Agent**.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que sigue seleccionado **Claude Sonnet 4.5**.
5. Envía este prompt:

   ```plaintext
   Actualiza el sitio para que permita filtrar por editorial y categoría según los requisitos de la incidencia correspondiente de GitHub. Ya existe una función auxiliar para las editoriales del ejercicio anterior; conserva y perfecciona ese trabajo si hace falta y completa la capa de datos, la interfaz y las pruebas restantes. Asegúrate de que todas las pruebas pasan. El servidor ya está en ejecución, así que no hace falta iniciarlo.
   ```

6. Observa cómo Copilot explora el proyecto, localiza la capa de datos, la interfaz y las pruebas, y modifica los archivos.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot puede tardar unos minutos en explorar, editar archivos y ejecutar pruebas. Mientras trabaja, pueden aparecer los controles **Keep** y **Undo**; espera a que termine antes de decidir qué cambios conservar.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de confirmación para ejecutar pruebas en Copilot Chat](../../../_images/ex3-agent-mode-run-tests.png)

8. Si algunas pruebas fallan durante el proceso, deja que Copilot itere y corrija los errores.
9. Explora el código generado y revisa los posibles problemas.

   ![Sesión completa de Copilot Agent Mode](../../../_images/ex3-agent-mode-proposed-changes.png)

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de inteligencia artificial (IA).

10. Vuelve al navegador con el sitio en ejecución y prueba la nueva funcionalidad.
11. Cuando hayas revisado el código y confirmado que funciona, selecciona **Keep** en Copilot Chat.

## Resumen y siguientes pasos

Has explorado cómo el modo agente de GitHub Copilot añade funcionalidades a Tailspin Toys. En este ejercicio:

- Aprendiste cómo el modo agente implementa funcionalidades en la interfaz y la capa de datos.
- Aprendiste cómo Copilot explora el proyecto, localiza archivos y coordina cambios.
- Revisaste los cambios y las pruebas generados antes de combinarlos.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementar la paginación

La lista de juegos puede crecer y necesitar paginación. Usa las habilidades de este ejercicio y pide a Copilot que la implemente. Sigue las prácticas recomendadas, usa los archivos de instrucciones, decide si el tamaño de página será fijo o configurable y proporciona el contexto necesario. Es normal iterar y pedir cambios.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers
