---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Muchas organizaciones tienen deuda técnica: problemas de seguridad pendientes, código heredado que hay que actualizar o funcionalidades relegadas en el backlog por falta de tiempo. El agente de GitHub Copilot en la nube puede actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así puedes delegar tareas y centrarte en otras prioridades.

En este ejercicio explorarás cómo:

- Personalizar el entorno en el que se genera código.
- Asegurar que las operaciones se realizan de forma segura.
- Delimitar correctamente las incidencias.
- Asignar incidencias a Copilot.

## Escenarios

Tailspin Toys quiere solucionar dos tareas. Primero, añadir comentarios de TypeScript Documentation (TSDoc) a todas las funciones exportadas de la aplicación. También quiere mostrar juegos relacionados (otros títulos de la misma categoría) en la página de detalles para facilitar que los visitantes descubran más juegos; esta implementación inicial servirá para probar la experiencia de usuario (UX).

Son tareas que pueden quedar relegadas y que el agente en la nube puede abordar de forma asíncrona mientras tú te ocupas de otras prioridades.

## Introducción al agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube][copilot-agents] trabaja en segundo plano, de forma parecida a un desarrollador. Puedes, por ejemplo, [asignarle una incidencia de GitHub][assign-issue]. El agente crea una PR en borrador para hacer seguimiento del trabajo, configura un entorno y empieza a trabajar. Puedes consultar la sesión mientras está en curso o después. Cuando la solución esté lista para revisarse, el agente te mencionará en la PR.

## La importancia de delimitar bien las instrucciones

GitHub Copilot no elimina la necesidad de pensar. Incluso las tareas aparentemente sencillas pueden tener detalles complejos; por eso, [delimita con cuidado el trabajo que asignas al agente en la nube][cloud-agent-best-practices]. Trátalo como un compañero de programación con inteligencia artificial (IA): trabaja por etapas, aprende, experimenta y adapta el proceso. Los fundamentos del desarrollo no cambian por usar IA generativa.

## Instrucciones personalizadas del repositorio

Antes de asignar trabajo, revisa en modo de solo lectura los archivos de instrucciones del repositorio para que puedas observar después su efecto.

1. En la interfaz web de GitHub o en un Codespace, abre `.github/copilot-instructions.md` y revisa la sección **Code standards**, especialmente las convenciones de TypeScript y los comentarios TSDoc.
2. Abre `.github/instructions/unit-tests.instructions.md` y observa cómo el frontmatter `applyTo` limita las instrucciones a los archivos `**/*.test.ts`.

Al asignar la incidencia `Falta documentación en el código`, comprueba si la PR resultante incluye comentarios TypeScript Documentation (TSDoc), convenciones de TypeScript y encabezados de comentarios procedentes de estas instrucciones.

## Configura el entorno de desarrollo del agente en la nube

El agente en la nube usa [GitHub Actions][github-actions] para preparar el entorno. El flujo de trabajo especial `.github/workflows/copilot-setup-steps.yml` puede instalar las herramientas y dependencias que necesita el agente. El proyecto ya incluye este flujo para proporcionar Node.js, las dependencias, los navegadores para las pruebas de extremo a extremo y la base de datos SQLite migrada y poblada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Asigna a tus pasos los permisos mínimos necesarios. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Para clonar el repositorio durante la configuración, por ejemplo para instalar dependencias, necesitas el permiso `contents: read`. Si no lo clonas aquí, Copilot lo hará automáticamente cuando terminen estos pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (toda la aplicación usa Astro y Drizzle/libSQL).
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos en SQLite local para que las compilaciones y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo funciona como otros flujos de GitHub Actions, pero hay dos aspectos importantes:

- Tiene un único trabajo llamado `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la PR.
- El desencadenador `workflow_dispatch` permite ejecutar el flujo manualmente desde la pestaña **Actions** para comprobarlo.

## Añade documentación

La falta de documentación ralentiza el trabajo y dificulta el mantenimiento. Asignar esta deuda técnica al agente en la nube permite que genere documentación en segundo plano. Más adelante revisarás su trabajo.

1. En una pestaña nueva, ve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues** y, después, **New issue**.
3. Selecciona **Blank issue**.
4. En **Title**, escribe `Falta documentación en el código`.
5. En **Description**, pega:

   ```plaintext
   Nuestra organización exige que las funciones y los métodos incluyan comentarios TSDoc cuando sean útiles. Sin embargo, las últimas actualizaciones no han seguido esta norma. Hay que actualizar el código existente para incluir comentarios donde ayuden a aclarar el comportamiento.
   ```

6. Selecciona **Create**.
7. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

   ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

8. Selecciona **Assign**, abre la pestaña **Pull Requests** y selecciona la solicitud nueva, cuyo título será parecido a `[WIP]: Falta documentación en el código`. Si no aparece, espera un momento y actualiza la página.
9. Espera a que Copilot cree una lista de tareas. Puede tardar varios minutos mientras configura el entorno, analiza el proyecto y planifica el trabajo.
10. Revisa las tareas y el progreso. En la cronología de la PR, selecciona **View session** para ver la sesión.

   ![Sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que tengas que actualizar la página para ver el indicador actualizado.

La sesión muestra cómo Copilot explora el código, analiza el estado del proyecto, planifica y realiza cambios. Mientras trabaja, aprovecharás su ejecución asíncrona para asignarle otra tarea.

## Añade una sección de juegos relacionados a la página de detalles

Asigna también al agente la tarea de ayudar al equipo de diseño a explorar los juegos relacionados.

1. Vuelve al repositorio en github.com, selecciona **Issues** y, después, **New issue**.
2. Selecciona **Blank issue**.
3. En **Title**, escribe `Mostrar juegos relacionados en la página de detalles`.
4. En **Description**, pega:

   ```markdown
   Queremos ayudar a los visitantes a descubrir más juegos mostrando títulos relacionados en cada página de detalles. El equipo de diseño quiere explorar la experiencia de usuario (UX) y realizar pruebas de aceptación. Los requisitos son:

   - Añade en `src/lib/` una función auxiliar de acceso a datos que devuelva los demás juegos de la misma categoría que uno determinado, sin incluir ese juego.
   - Muestra una sección «Related games» en la página de detalles del juego y utiliza la función auxiliar.
   - Gestiona el caso en que un juego no tenga juegos relacionados.
   - Crea pruebas unitarias para la nueva función auxiliar.
   - Antes de crear la solicitud de incorporación de cambios, confirma que se superen todas las pruebas.
   ```

5. Selecciona **Create** y, a la derecha, **Assign to Copilot**.

   ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

6. En el cuadro de asignación, selecciona **Assign**.

Poco después verás un emoji 👀 en el primer comentario de la incidencia, que indica que Copilot ha empezado a trabajar.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

No es necesario supervisar constantemente la tarea; podrás revisar el resultado cuando esté listo. Mientras tanto, pasa a crear y usar agentes personalizados.

## Resumen y siguientes pasos

Has explorado el agente de GitHub Copilot en la nube. Puedes asignarle incidencias para abordar deuda técnica, crear funcionalidades o ayudar a migrar código. En este ejercicio:

- Personalizaste el entorno de generación de código.
- Aprendiste cómo se protegen las operaciones.
- Exploraste la importancia de delimitar las incidencias.
- Asignaste incidencias a Copilot.

Mientras el agente trabaja en segundo plano, puedes [crear y usar agentes personalizados][next-lesson]. El agente en la nube también puede usar servidores de Model Context Protocol (MCP) y las instrucciones personalizadas que exploraste antes.

## Recursos

- [Acerca del agente de Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
