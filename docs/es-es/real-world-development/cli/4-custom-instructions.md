---
title: "Lección 4 - Guiar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código del filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental cuando trabajas con inteligencia artificial (IA) generativa. Si una tarea debe realizarse de una forma concreta, conviene que Copilot disponga de esa orientación. Los [archivos de instrucciones][instruction-files] describen tanto qué código quieres como cómo debe estructurarse. Ahora que ya has creado el filtrado, explorarás las instrucciones que ha usado Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección, harás lo siguiente:

- Explorarás cómo llegan al agente las instrucciones del repositorio y las instrucciones asociadas a rutas.
- Actualizarás el archivo de instrucciones para que se respeten los estándares de programación.
- Observarás el efecto de los archivos de instrucciones en el código.

## Escenario

Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo:

- Los comentarios deben explicar la intención y las decisiones que no sean obvias, no repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, parámetros y valores devueltos con TypeScript Documentation (TSDoc) o JavaScript Documentation (JSDoc), incluido un argumento `db` inyectable cuando corresponda.
- Los componentes Astro reutilizables deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambia el código relacionado.
- Deben conservarse las directrices de formato y lint existentes.

Los archivos de instrucciones ayudan a que Copilot tenga el contexto necesario para realizar las tareas de acuerdo con estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que entienda mejor el estilo de programación y los requisitos. Puedes especificar convenciones, bibliotecas y preferencias sobre los comentarios. Las instrucciones pueden aplicarse a todo el repositorio o a determinados tipos de archivos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un archivo único que se envía a Copilot en todas las solicitudes del repositorio. Debe incluir información relevante para la mayoría de las solicitudes.
- `.github/instructions/*.instructions.md`, archivos con directrices para determinados lenguajes, tipos de archivos o tareas.

> [!NOTE]
> Los formatos de instrucciones y su compatibilidad varían según el entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explora los archivos de instrucciones personalizados del proyecto

El proyecto de ejemplo ya incluye archivos de instrucciones. Revísalos antes de hacer cambios para comprobar su efecto.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces, no en el terminal, abre `.github/copilot-instructions.md`.
3. Revisa la breve descripción del proyecto y las directrices de programación. Se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y revisa los archivos. Hay instrucciones para Astro, la capa de datos Drizzle, las pruebas y otros ámbitos.
5. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` del principio, que establece el patrón de archivos al que se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa los enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir instrucciones extensas en otras más pequeñas y reutilizables, y enlazar ejemplos.

## Actualiza los archivos de instrucciones según las directrices del equipo

Los archivos existentes son un buen punto de partida, pero falta una directriz. Modifica el archivo principal para que se generen comentarios TSDoc en TypeScript.

1. En `.github/copilot-instructions.md`, busca la sección **Code formatting guidance**, aproximadamente en la línea 35.
2. Añade como último elemento de la lista:

   ```markdown
   - Todo el código TypeScript nuevo debe incluir comentarios TSDoc con fines de documentación.
   ```

El archivo se guardará automáticamente.

## Usa las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al empezar una conversación. Después de guardarlas, sal de la conversación y reanuda la sesión de filtrado para que Copilot cargue las nuevas instrucciones sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit`. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está activa la rama `game-filters-cli`.
3. Pide a Copilot que aplique las instrucciones actualizadas:

   ```plaintext
   Acabamos de actualizar las instrucciones y las directrices de código. ¿Puedes actualizar el código que has generado para que las cumpla?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Comprueba que los comentarios TSDoc nuevos describen el código con precisión.

## Resumen y siguientes pasos

Has explorado cómo Copilot CLI obtiene el contexto de los archivos de instrucciones y has aplicado un estándar nuevo a la funcionalidad. En concreto:

- Exploraste cómo llegan al agente las instrucciones del repositorio y las instrucciones asociadas a rutas.
- Actualizaste el archivo de instrucciones para que se respeten los estándares de programación.
- Observaste el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable quality-checks][next-lesson] para aplicar las pruebas y el análisis de lint de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
