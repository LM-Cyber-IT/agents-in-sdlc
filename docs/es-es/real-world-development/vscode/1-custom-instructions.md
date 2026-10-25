---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Requisitos previos][previous-lesson] · [Siguiente ejercicio: Modo agente →][next-lesson]

El contexto es fundamental cuando trabajas con inteligencia artificial (IA) generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita información previa, asegúrate de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] proporcionan directrices para generar código.

En este ejercicio, harás lo siguiente:

- Explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y las instrucciones asociadas a rutas.
- Generarás la primera parte de los datos del filtrado (una función auxiliar para las editoriales) con las instrucciones actuales.
- Añadirás un estándar general del repositorio a `.github/copilot-instructions.md`.
- Volverás a enviar el mismo prompt y observarás cómo el código generado adopta el nuevo estándar.
- Confirmarás en `main` la actualización de instrucciones y la parte de filtrado para que el siguiente ejercicio pueda aprovecharlas.

> [!CAUTION]
> El código generado puede no seguir todas las normas especificadas. Copilot es no determinista. El objetivo es observar la tendencia después de actualizar las instrucciones, no obtener una salida idéntica carácter por carácter.

## Archivos de instrucciones

### Escenario

Tailspin Toys sigue varias directrices de desarrollo:

- La capa de datos debe tener pruebas unitarias.
- La interfaz debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en comentarios TypeScript Documentation (TSDoc).
- Cada archivo debe empezar con un bloque de comentarios que describa su función.

Usarás archivos de instrucciones para proporcionar a Copilot el contexto necesario para trabajar de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas ayudan a Copilot a entender el estilo de programación y los requisitos. Puedes indicar convenciones, bibliotecas y preferencias sobre los comentarios. Las instrucciones pueden aplicarse a todo el repositorio o solo a determinados tipos de archivos o tareas.

Hay dos tipos de archivos:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en todas las solicitudes del repositorio. Debe contener información general relevante para las solicitudes de chat y CLI, como la pila tecnológica, la estructura y las prácticas recomendadas.
- `.github/instructions/*.instructions.md`, archivos que proporcionan directrices para lenguajes, tipos de archivo o tareas concretos, como componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el entorno de desarrollo integrado (IDE), los archivos de instrucciones se usan para generar código en Copilot Chat, no para las sugerencias de finalización ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube usan las instrucciones del repositorio y los archivos `*.instructions.md` con frontmatter `applyTo`.
>
> Copilot también admite otros estándares, como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Los ejemplos del proyecto muestran un enfoque posible:

- Mantén `copilot-instructions.md` centrado en las directrices generales, como la descripción y la estructura del proyecto y los estándares comunes.
- Usa `*.instructions.md` para tareas o tipos de archivo concretos, como pruebas unitarias, componentes Astro o la capa de datos.
- Escribe instrucciones claras en lenguaje natural e incluye ejemplos de código correcto e incorrecto.

No hay una única forma de crear archivos de instrucciones. La experimentación permite descubrir qué funciona mejor en cada proyecto.

> [!TIP]
> Revisa las instrucciones de [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions] del proyecto. Copilot también puede ayudarte a generar archivos de instrucciones; cada entorno ofrece esa función de una forma distinta. Para encontrar plantillas, consulta [awesome-copilot][awesome-copilot].

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizados del proyecto

Revisa el archivo principal `copilot-instructions.md` y los archivos `*.instructions.md` del repositorio.

1. Abre `.github/copilot-instructions.md`.
2. Revisa la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de GitHub Actions.
3. Abre `.github/instructions` y revisa las instrucciones para Astro, la capa de datos Drizzle, las pruebas y otros temas.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` define un patrón relativo a la raíz del repositorio; por ejemplo, `**/*.test.ts` coincide con los archivos de pruebas TypeScript.
5. Revisa las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa al final los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación, pero aún no requiere documentación en el código. Añadirás comentarios TSDoc y encabezados de comentarios en los pasos siguientes.

## Crea una rama para los cambios

Crea una rama nueva para separar el trabajo de este ejercicio.

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al Codespace, abre el panel Copilot Chat y selecciona **New Chat**. El modo y el modelo dependen del ejercicio; cada paso los especificará cuando sea necesario.

Primero envía un prompt con las instrucciones actuales. Después las actualizarás y enviarás el mismo prompt para comparar el resultado.

1. Cierra las pestañas del editor que tengas abiertas para que Copilot reciba solo el contexto necesario.
2. Abre `src/lib/publishers.ts` para indicar dónde debe estar la función auxiliar.
3. En la vista Chat, selecciona **Agent** en el selector de agentes para que Copilot pueda modificar archivos.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con una función auxiliar de acceso a datos que devuelva todas las editoriales, con el nombre y el id de cada una. Aplica los cambios en los archivos.
   ```

5. Copilot explorará el proyecto y aplicará cambios, normalmente en el archivo auxiliar y sus pruebas.
6. Observa que la función propuesta está tipada, recibe el cliente `db` como primer argumento y devuelve una matriz tipada de editoriales; estas características proceden de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa si faltan comentarios TSDoc y un encabezado de comentarios para el archivo.

> [!CAUTION]
> Copilot podría añadir comentarios aunque no se lo hayas pedido. No pasa nada: el objetivo es observar una mejora coherente tras actualizar las instrucciones.

## Añade un estándar general del repositorio

El archivo `.github/copilot-instructions.md` proporciona a Copilot contexto general del proyecto. Añade un estándar para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, aproximadamente en la línea 27. Todavía no incluye reglas de documentación en el código.
3. Añade estas líneas Markdown debajo de los estándares existentes:

   ```markdown
   - Cada función exportada debe incluir un comentario TSDoc que describa su propósito, sus parámetros y su valor devuelto.
   - Antes de las importaciones o de cualquier código, añade un bloque de comentarios que explique el propósito del archivo.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Usa `.github/copilot-instructions.md` para los estándares generales del proyecto y archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos.

## Vuelve a enviar el prompt y observa el cambio

Vuelve a generar la misma función auxiliar para comprobar si Copilot aplica el estándar que acabas de añadir.

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contexto de la conversación.
2. Abre `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía el mismo prompt:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con una función auxiliar de acceso a datos que devuelva todas las editoriales, con el nombre y el id de cada una. Aplica los cambios en los archivos.
   ```

4. Observa que el archivo propuesto empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Funciones auxiliares de acceso a datos de editoriales para la plataforma de financiación colectiva Tailspin Toys.
    * Proporciona funciones para recuperar información de editoriales de la base de datos.
    */
   ```

5. Comprueba que la función incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su id y su nombre.
    *
    * @param db - Cliente de base de datos de Drizzle.
    * @returns Una promesa que se resuelve en una matriz de objetos de editorial.
    */
   ```

Has guiado a Copilot para que siga un estándar nuevo y lo aplique al código que utilizarás en el siguiente ejercicio.

## Confirma los cambios y publica la rama

Los archivos de instrucciones se gestionan mediante el control de código fuente como cualquier otro archivo.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Comprueba que están presentes la actualización de instrucciones y los cambios de la función auxiliar:

   ```bash
   git status
   ```

3. Prepara y confirma el archivo de instrucciones y todos los archivos que Copilot haya cambiado para la función auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios necesarios (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Vincula la rama publicada con la incidencia de estándares de documentación del backlog. Copilot puede buscar la incidencia y crear una solicitud de incorporación de cambios (PR) asociada.

1. Abre Copilot Chat en el Codespace y selecciona **New Chat**.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que está seleccionado **Agent**.
3. Pide a Copilot que busque la incidencia y cree una PR desde la rama actual:

   ```plaintext
   Busca la incidencia relacionada con la actualización del archivo de instrucciones. Crea una solicitud de incorporación de cambios desde la rama actual, indica que cierra esa incidencia y menciona también que hemos añadido la función auxiliar de editoriales para el próximo trabajo de filtrado.
   ```

4. Si Copilot pide permiso para usar herramientas de GitHub, revisa la operación y selecciona **Allow** cuando corresponda.
5. Cuando se cree la PR, pide a Copilot que la combine y que vuelva a `main`:

   ```plaintext
   Combina la PR en main. Después, vuelve localmente a main y descarga el código más reciente para que el repositorio esté actualizado.
   ```

6. Si Copilot pide permiso para usar herramientas de GitHub o ejecutar comandos del terminal, revisa la operación y selecciona **Allow** cuando corresponda.

¡Has creado y combinado una solicitud de incorporación de cambios con GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene el contexto de los archivos de instrucciones y has usado Copilot Chat en VS Code para:

- Enviar un prompt de generación de código y observar el resultado con las instrucciones existentes.
- Añadir un estándar general a `.github/copilot-instructions.md`.
- Volver a enviar el mismo prompt y observar cómo el código adopta el nuevo estándar.
- Confirmar y combinar en `main` la actualización de instrucciones y la función auxiliar para que el siguiente ejercicio pueda usarlas.

A continuación, pondrás esas instrucciones en práctica con el [modo agente][next-lesson] para que Copilot añada una funcionalidad en distintas partes del código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
