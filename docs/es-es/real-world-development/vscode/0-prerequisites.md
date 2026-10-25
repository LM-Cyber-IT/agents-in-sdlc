---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios de VS Code, prepara el entorno. Crearás una copia del repositorio Tailspin Toys, iniciarás un [Codespace][codespaces] y confirmarás que GitHub Copilot Chat funciona en el editor.

## Configura el repositorio del taller

Crearás una copia del repositorio a partir de la [plantilla][template-repository]. La copia incluirá los archivos necesarios para el taller.

1. En una ventana nueva del navegador, ve a la [plantilla de Tailspin Toys][tailspin-template].
2. Selecciona **Use this template** y, después, **Create a new repository**.

   ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si participas en un evento organizado por GitHub o Microsoft, sigue las instrucciones de los mentores. Si no, crea el repositorio en una organización en la que tengas acceso a GitHub Copilot.

   ![Configuración del repositorio creado a partir de la plantilla](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio que has creado (`organization-or-user-name/repository-name`); la necesitarás más adelante.

> [!NOTE]
> **El backlog está listo**
>
> Al crear el repositorio a partir de la plantilla, se crea automáticamente un backlog de incidencias de GitHub. Las usarás durante el taller; no hace falta que crees ninguna.

## Crea un Codespace

Usarás un Codespace para completar los ejercicios. [GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube para escribir, ejecutar y depurar código desde el navegador. Proporciona un entorno de desarrollo integrado (IDE) completo con compatibilidad para varios lenguajes de programación, extensiones y herramientas.

1. Ve al repositorio que acabas de crear.
2. Selecciona el botón verde **Code**.

   ![Botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, **+** para crear un Codespace.

   ![Crear un Codespace](../../../_images/ex0-create-codespace.png)

La creación puede tardar varios minutos, aunque es más rápida que instalar manualmente todos los servicios. Mientras tanto, puedes explorar otras funciones de GitHub Copilot.

> [!CAUTION]
> Volverás al Codespace en un ejercicio posterior. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> Este taller está diseñado para ejecutarse en un Codespace o en un [contenedor de desarrollo][dev-containers] local. Ambos incluyen los requisitos previos necesarios. Para trabajar en local, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando se te solicite. VS Code creará el mismo contenedor que usa Codespaces.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Usa GitHub Copilot Chat y el modo agente

Para acceder al modo agente de GitHub Copilot Chat, debes tener instalada la extensión GitHub Copilot Chat en el IDE. Ya debería estar instalada si usas un Codespace de GitHub.

> [!TIP]
> Si no tienes la extensión GitHub Copilot Chat, [instálala desde Visual Studio Code Marketplace][copilot-chat-extension] o abre la vista **Extensions** de Visual Studio Code, busca **GitHub Copilot Chat** y selecciona **Install**.

Una vez instalada la extensión, quizá tengas que autenticarte con tu cuenta de GitHub.

1. Vuelve a tu Codespace.
2. Si Copilot Chat no aparece a la derecha del editor, selecciona el icono **Copilot Chat** en la parte superior del Codespace.
3. Escribe un mensaje, como «Hola», y pulsa <kbd>Enter</kbd> para activar Copilot Chat.
4. Si no has iniciado sesión, sigue las instrucciones para autenticarte con tu cuenta de GitHub.

   ![Ejemplo del mensaje de autenticación de Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Después de autenticarte, debería aparecer la ventana Copilot Chat.

## Resumen

¡Enhorabuena! Has creado una copia del repositorio del taller y has empezado a crear el Codespace que usarás para programar.

## Siguiente paso

Empieza a trabajar con Copilot. Continúa con [Instrucciones personalizadas][next-lesson] para enseñarle las convenciones del proyecto.

## Recursos

- [Información general sobre GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Primeros pasos con Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
