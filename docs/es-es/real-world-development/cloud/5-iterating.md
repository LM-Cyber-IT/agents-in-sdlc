---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Supervisar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Has usado GitHub Copilot para documentar el código, añadir juegos relacionados y crear funciones de accesibilidad: modos de alto contraste y claro. Ahora revisarás los cambios y, si hace falta, pedirás a Copilot que los mejore.

### Escenario

El diseño de software y DevOps sigue requiriendo un proceso de revisión, aunque se use inteligencia artificial (IA) generativa. Revisarás las propuestas del agente para la documentación, los juegos relacionados y la accesibilidad antes de involucrar al resto del equipo.

## Seguridad y agente de GitHub Copilot en la nube

El agente en la nube trabaja de forma asíncrona, con límites de seguridad:

- Copilot tiene acceso de lectura al repositorio y acceso de escritura solo a la rama de trabajo.
- Se ejecuta en GitHub Actions, en un entorno efímero e independiente.
- Una persona debe aprobar la ejecución de los flujos de trabajo de GitHub Actions.
- [El acceso a recursos externos está limitado de forma predeterminada][agent-firewall], incluidos los servidores de Model Context Protocol (MCP).

## Revisa la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) del agente, que añade documentación al código.

> [!NOTE]
> Es posible que veas un aviso que indica que el firewall bloqueó a GitHub Copilot. Es lo esperado: por defecto, el acceso del agente a recursos externos, incluidos los servidores MCP, es limitado. Si quieres, puedes [personalizar o desactivar el firewall del agente][agent-firewall].

1. Ve a tu repositorio en github.com.
2. Selecciona **Pull Requests** y abre la PR titulada, por ejemplo, **Añadir documentación que falta**.
3. Si la PR aún incluye la marca **[WIP]**, espera a que Copilot termine; puede tardar unos minutos.
4. Selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Busca comentarios de TypeScript Documentation (TSDoc), convenciones de TypeScript y encabezados de comentarios. Proceden de los archivos de instrucciones que revisaste al principio del ejercicio 2.
6. Vuelve a **Conversation** y desplázate hacia abajo.
7. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Confirma que los flujos se han puesto en cola en la sección de comprobaciones de la PR. Las comprobaciones del proyecto para la aplicación Astro deben finalizar correctamente; pueden tardar unos minutos.

## Pide cambios a GitHub Copilot

Puedes mencionar a Copilot en los comentarios de la PR o en comentarios en línea del código para pedirle cambios. Copilot iniciará otra sesión para resolverlos. Las propuestas pueden variar; por ejemplo, puedes pedir que:

- Añada encabezados breves a los archivos de código.
- Añada comentarios TSDoc a los archivos TypeScript y Astro.
- Cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario en la PR y menciona a **@copilot**. Usa una de las ideas anteriores o pide otro cambio de documentación.
2. Selecciona **View Session** para ver cómo Copilot inicia otra sesión y realiza los cambios.
3. Si quieres volver a la PR, selecciona **Back to pull request**.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, comprueba que hay una confirmación nueva en la PR.
5. Selecciona **Files changed** y revisa los cambios.

Itera hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y combinarla.

![Marcar la solicitud de incorporación de cambios como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Revisa la PR del agente para mostrar juegos relacionados en la página de detalles.

1. Ve al repositorio en GitHub.com y selecciona **Pull Requests**.
2. Abre la PR titulada, por ejemplo, **Mostrar juegos relacionados en la página de detalles**.
3. Selecciona **Files changed** y revisa el código generado.
4. Cuando termines, vuelve a **Conversation**. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.
5. Comprueba que las verificaciones del proyecto terminan correctamente.
6. **Opcional:** para probar la funcionalidad manualmente en Codespaces, abre un terminal y cambia `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Puedes pedir cambios con otro comentario en **Conversation**; por ejemplo, ajustar la cantidad de juegos relacionados o añadir los encabezados y comentarios TSDoc. Recuerda que esta tarea se asignó antes de actualizar las instrucciones personalizadas.

## Revisa las funciones de accesibilidad

Por último, revisa la PR del agente de accesibilidad. Debe incluir el modo de alto contraste que pediste en el ejercicio 3 y el modo claro que solicitaste en el ejercicio 4.

1. Ve al repositorio en GitHub.com, selecciona **Pull Requests** y abre la PR titulada, por ejemplo, **Añadir un modo de alto contraste al sitio**.
2. Si aún aparece la marca **[WIP]**, espera a que Copilot termine; puede tardar unos minutos.
3. Selecciona **Files changed** y revisa:
   - Los controles de la interfaz para cambiar de modo.
   - El almacenamiento local que conserva las preferencias del usuario.
   - Los cambios de hojas de estilo en cascada (CSS) o de estilos para los modos de alto contraste y claro.
   - Los atributos de Accessible Rich Internet Applications (ARIA) y la navegación con teclado.
   - El código JavaScript/TypeScript que gestiona el cambio de modo.
4. Vuelve a **Conversation**. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.
5. Confirma que las comprobaciones del proyecto terminan correctamente.
6. **Opcional:** cambia a la rama del agente en Codespaces:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

7. Inicia la aplicación y prueba en el navegador los controles de alto contraste y modo claro. Confirma que funcionan y que las preferencias se conservan al recargar la página.

Si detectas problemas o mejoras de accesibilidad, menciona a **@copilot** en un comentario para solicitar cambios.

## Ejercicio opcional: sigue delegando

El agente en la nube es útil para tareas del backlog que puedes delegar mientras haces otro trabajo. Para practicar, crea incidencias nuevas y asígnalas a Copilot. Algunas ideas:

- Crear un formulario de interés para patrocinadores en la página de detalles.
- Implementar la paginación en la lista de juegos.
- Añadir validación de entradas y gestión de errores a las funciones auxiliares de acceso a datos.

## Resumen

Has completado el itinerario del agente en la nube. En estos ejercicios:

- Revisaste las instrucciones personalizadas del repositorio para conocer su efecto en el agente.
- Asignaste incidencias al agente de Copilot en la nube y observaste cómo configuraba el entorno, planificaba y trabajaba de forma asíncrona.
- Creaste y usaste un agente personalizado de accesibilidad para añadir los modos de alto contraste y claro.
- Usaste la página de agentes de Copilot para supervisar y dirigir la sesión de accesibilidad.
- Revisaste e iteraste sobre las solicitudes de incorporación de cambios del agente, mencionaste a `@copilot` para pedir cambios y aprobaste flujos de trabajo.

## Repaso y siguientes pasos

Has completado el itinerario del agente en la nube. Los otros entornos complementan lo que has practicado:

- 🖥️ **[Entorno de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración con Model Context Protocol (MCP) desde el entorno de desarrollo integrado (IDE).
- 💻 **[Entorno de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con Plan y Autopilot, habilidades de agente, un agente QA, Playwright MCP, Agent Merge y la delegación opcional con `/delegate`.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente una incidencia de refactorización o de cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube][agent-firewall]

| [← Ejercicio anterior: Supervisar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
