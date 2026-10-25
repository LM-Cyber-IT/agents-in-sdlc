---
title: "Lección 5 - Personalizar y usar la habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato del informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Desarrollar no consiste solo en escribir código. Ya has comprobado manualmente que el código funciona y has usado archivos de instrucciones para que respete los estándares. ¿Y las pruebas, el lint y los demás elementos de la integración continua (CI)?

Para este tipo de tareas, las **habilidades de agente** son una buena opción. Ayudan a Copilot a entender cómo ejecutar operaciones como estas.

En esta lección, harás lo siguiente:

- Explorarás la habilidad `quality-checks` existente.
- Personalizarás el formato de sus resultados.
- Recargarás y ejecutarás la habilidad.

## Escenario

Tailspin Toys utiliza la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que sea más fácil de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos de apoyo que el agente carga cuando los necesita. Una habilidad es una carpeta cuyo archivo Markdown principal se llama `SKILL.md`. El archivo contiene un frontmatter con el nombre y la descripción, una presentación de la habilidad y orientación sobre cuándo debe usarse. La carpeta también puede incluir subcarpetas con scripts y otros recursos.

> [!NOTE]
> Una habilidad no necesita otros archivos ni carpetas. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md`, porque usa los comandos existentes del proyecto.

Las habilidades pueden residir en `.github/skills` para que el equipo las comparta y reutilice, o en la carpeta de habilidades del usuario, `~/.copilot/skills`.

## Explora la habilidad

Explora la habilidad `quality-checks` que creó el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` del principio. La descripción ayuda a Copilot a decidir cuándo usar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot durante las pruebas y el análisis de lint.
4. Comprueba que la habilidad todavía no incluye una sección para dar formato al informe de resultados.

## Ejecuta la habilidad antes de cambiarla

Pide a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Usa este prompt:

   ```plaintext
   Ejecuta la habilidad quality-checks para las pruebas unitarias, lint y las comprobaciones de tipos.
   ```

3. Lee el informe final.

## Personaliza el informe

Mejora el informe para que indique qué se ha ejecutado, si se ha completado correctamente y qué han comunicado las herramientas.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Formato de salida de los resultados

   Al terminar, indica qué comandos se han ejecutado y si han terminado correctamente, han fallado o se han bloqueado (`passed`, `failed` o `blocked`). Incluye el número de pruebas, la duración, los errores, las advertencias y otras métricas solo si las herramientas las comunican. Indica el siguiente paso ante cada fallo o bloqueo y no presentes nunca una comprobación omitida o incompleta como superada.
   ```

3. El archivo se guarda automáticamente.

## Ejecuta la habilidad actualizada

Recarga la habilidad para que Copilot use el formato actualizado y compara el nuevo informe con el primero.

Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación. Comprueba el nuevo formato del informe.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Usa exactamente el mismo prompt que antes:

   ```plaintext
   Ejecuta la habilidad quality-checks para las pruebas unitarias, lint y las comprobaciones de tipos.
   ```

3. Lee el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y usado una habilidad de agente existente. En esta lección:

- Exploraste la habilidad `quality-checks` para las pruebas unitarias, lint y las comprobaciones de tipos.
- Personalizaste el formato de sus resultados.
- Recargaste y ejecutaste la habilidad.

Ese cambio acompañará al filtrado en la PR de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio mediante el [servidor Playwright MCP][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad son referencias, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación: `drawio`][drawio-example]
- [Pruebas del navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
