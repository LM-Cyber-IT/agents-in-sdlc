---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot son asistentes de inteligencia artificial (IA) especializados para tareas o ámbitos concretos del flujo de desarrollo. Sus archivos Markdown, guardados en `.github/agents`, proporcionan instrucciones, prácticas recomendadas, patrones de código y conocimientos del dominio. Así, un equipo puede convertir su experiencia en agentes reutilizables: por ejemplo, uno de accesibilidad que aplique las [pautas de accesibilidad para el contenido web (WCAG)][wcag], uno de seguridad o uno de pruebas.

Los agentes personalizados se definen mediante archivos Markdown en `.github/agents` o, de forma global, en `~/.copilot/agents`. Cada archivo incluye frontmatter YAML con, como mínimo, `name` y `description`, seguido de instrucciones Markdown que describen el comportamiento y los conocimientos del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se parecen porque ambos usan archivos Markdown y explican a la IA cómo realizar tareas. Una forma sencilla de distinguirlos: el **agente personalizado** es quien trabaja y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes. En este taller, el agente de accesibilidad revisará y actualizará el sitio según las pautas de accesibilidad; también podría usar habilidades para gestionar solicitudes de incorporación de cambios o pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como con cualquier función de IA, prueba e itera para encontrar lo que funciona en tu entorno y tus escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

En este ejercicio aprenderás cómo se definen los agentes personalizados y cómo asignarles una tarea.

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todos. Algunos usuarios han señalado que el tema oscuro actual es difícil de leer porque el contraste entre el texto y el fondo es insuficiente. El equipo de diseño ha pedido un modo de alto contraste que se pueda activar y desactivar. Usarás un agente personalizado para generar la funcionalidad.

## Revisa el agente personalizado de accesibilidad

Ya hay un agente personalizado de accesibilidad. Revisa el archivo para saber cómo guía a Copilot.

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

3. Revisa las secciones siguientes: responsabilidades principales, prácticas recomendadas, ejemplos de lenguaje de marcado de hipertexto (HTML), hojas de estilo en cascada (CSS) y JavaScript, y errores habituales.

## Crea y asigna una incidencia

El centro de control es el espacio central para trabajar con agentes. Desde allí puedes asignar tareas al agente de Copilot en la nube, supervisarlas y proporcionar orientación. Asígnale una tarea para crear el modo de alto contraste.

1. Ve a tu repositorio.
2. Selecciona **Issues** y, después, **New issue**.
3. Selecciona **Blank issue**.
4. En **Title**, escribe `Añadir un modo de alto contraste al sitio`.
5. En **Description**, escribe:

   ```plaintext
   Necesitamos un modo de alto contraste para el sitio. Debe incluir un control que el usuario pueda activar y el ajuste debe conservarse en el almacenamiento local del navegador.
   ```

6. Selecciona **Create**.
7. A la derecha, selecciona **Assign to Copilot**.
8. En la lista de agentes personalizados, selecciona **Accessibility agent**.

   ![Cuadro de asignación del agente en la nube con el agente personalizado Accessibility seleccionado](../../../_images/ex5-select-custom-agent.png)

9. Selecciona **Assign**. Copilot empezará a trabajar en segundo plano.

## Resumen y siguientes pasos

Has explorado los agentes personalizados de GitHub Copilot, asistentes especializados para tareas y ámbitos concretos. Permiten codificar los conocimientos y estándares del equipo en agentes reutilizables. En este ejercicio:

- Aprendiste cómo se definen los agentes personalizados.
- Asignaste una tarea a un agente personalizado.

Mientras Copilot implementa el modo de alto contraste, [supervisarás y guiarás la sesión del agente][next-lesson] desde el centro de control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparar la organización para usar agentes personalizados][org-custom-agents]
- [Preparar la empresa para usar agentes personalizados][enterprise-custom-agents]

| [← Ejercicio anterior: Agente de GitHub Copilot en la nube][previous-lesson] | [Siguiente ejercicio: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
