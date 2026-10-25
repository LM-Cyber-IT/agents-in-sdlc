---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los agentes personalizados de GitHub Copilot son asistentes de inteligencia artificial (IA) especializados que se adaptan a tareas o ámbitos concretos del desarrollo. Se definen con archivos Markdown en `.github/agents` y pueden incluir instrucciones, prácticas recomendadas, patrones de código y conocimientos del dominio. Los equipos pueden convertir su experiencia en agentes reutilizables, como uno de accesibilidad que aplique las [pautas de accesibilidad para el contenido web (WCAG)][wcag], uno de seguridad o uno de pruebas.

Los agentes personalizados se definen mediante archivos Markdown en `.github/agents` o, de forma global, en `~/.copilot/agents`. Cada archivo incluye frontmatter YAML con `name` y `description`, seguido de un prompt Markdown que define el comportamiento, los conocimientos y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se parecen: ambos usan principalmente archivos Markdown y describen cómo realizar tareas. Una distinción útil es que el **agente personalizado** es quien trabaja y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes. En este taller, el agente personalizado de accesibilidad revisará y actualizará el sitio según las pautas de accesibilidad; para ello, podría usar habilidades de flujo de trabajo de solicitudes de incorporación de cambios o de gestión de pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Prueba e itera para encontrar lo que funciona en tu entorno y tus escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys quiere que la plataforma de financiación colectiva sea accesible para todos, independientemente de sus capacidades visuales o preferencias. Algunos usuarios indican que el tema oscuro es difícil de leer por el bajo contraste. El equipo de diseño ha pedido un modo de alto contraste que se pueda activar y desactivar. Usarás un agente personalizado para generar esta funcionalidad.

## Revisa el agente personalizado de accesibilidad

Ya hay un agente personalizado de accesibilidad. Revisa su contenido para saber cómo guía a Copilot.

1. Vuelve a tu Codespace y abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Revisa las secciones sobre responsabilidades principales, prácticas recomendadas, ejemplos de lenguaje de marcado de hipertexto (HTML), hojas de estilo en cascada (CSS) y JavaScript, y errores habituales.

## Usa el agente personalizado en Copilot Chat

VS Code muestra los agentes personalizados definidos en `.github/agents` en el selector de agentes de Copilot Chat. Selecciona uno para aplicar sus instrucciones y herramientas a una conversación.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al Codespace, abre el panel Copilot Chat y selecciona **New Chat**. El modo y el modelo dependen del ejercicio; cada paso los indicará cuando corresponda.

1. En el selector de agentes de la vista Chat, selecciona **Agent** si aún no está seleccionado.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Abre el selector de agentes, que muestra el agente activo, normalmente **default**.
3. Selecciona **Accessibility agent**.
4. Envía este prompt:

   ```plaintext
   Añade un modo de alto contraste al sitio. Debe incluir un control que el usuario pueda activar y el ajuste debe conservarse al recargar la página mediante el almacenamiento local del navegador.
   ```

5. Copilot explorará el código, propondrá cambios y los aplicará a los archivos del proyecto. Revisa las diferencias que aparecen en el chat.
6. Comprueba el indicador **Files changed** para ver los archivos modificados.

> [!NOTE]
> El proceso puede tardar unos minutos. Copilot puede cambiar componentes Astro, CSS y pruebas relacionadas.

Revisarás y guiarás el trabajo en curso en el siguiente ejercicio.

## Resumen y siguientes pasos

Has explorado los agentes personalizados de GitHub Copilot, asistentes de IA especializados para tareas y ámbitos concretos. Permiten codificar los conocimientos y estándares del equipo en agentes reutilizables. En este ejercicio:

- Revisaste un agente personalizado de accesibilidad.
- Usaste el agente de accesibilidad en Copilot Chat para implementar el modo de alto contraste.

A continuación, [supervisarás y guiarás el trabajo del agente][next-lesson], revisarás los cambios mientras se realizan y añadirás un control de modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

| [← Ejercicio anterior: Probar la funcionalidad con Playwright MCP][previous-lesson] | [Siguiente ejercicio: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
