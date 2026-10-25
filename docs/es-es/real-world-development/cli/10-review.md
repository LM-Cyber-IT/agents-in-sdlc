---
title: "Lección 10 - Repaso y pasos siguientes"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI en un flujo de trabajo continuo con Tailspin Toys. Has:

- Preparado un Codespace, instalado Copilot CLI, explorado el proyecto y localizado la incidencia de filtrado del backlog.
- Añadido valoraciones por estrellas, revisado el resultado en un navegador a través de un puerto reenviado de Codespaces y combinado manualmente tu primera solicitud de incorporación de cambios (PR).
- Partido de la incidencia de filtrado, definido el enfoque en modo Plan, desarrollado la funcionalidad en modo Autopilot y revisado el resultado en modo Interactive.
- Guiado al agente con instrucciones personalizadas y personalizado la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- Añadido el servidor Model Context Protocol (MCP) de Playwright y explorado el filtrado en un navegador real.
- Creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas del navegador.
- Revisado el cambio completo de filtrado y autorizado Agent Merge para su PR.
- Explorado comandos de barra para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Lo que has entregado

El taller tiene dos hitos de PR, cada uno en su propia rama creada desde `main` actualizado:

1. **Valoraciones por estrellas:** muestra en las tarjetas de juegos la valoración `starRating` existente y un estado explícito para los juegos sin valorar.
2. **Filtrado y flujo de calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe `quality-checks`, crea un perfil QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la apertura de su PR, has usado la misma conversación y la misma rama. Has agrupado el trabajo en una PR para simplificar el taller.

## Diferentes tipos de verificación

Has comprobado el código de varias formas: con pruebas automatizadas, inspeccionando el navegador y dejando que Copilot explore el navegador mediante MCP. La habilidad `quality-checks` ejecutó las pruebas unitarias, lint y comprobaciones de tipos, y las notificó con el nuevo formato. QA reunió esos resultados con la revisión de los requisitos y la cobertura de pruebas antes de abrir la PR.

Las pruebas nuevas deben cubrir carencias reales; una ejecución de QA que no necesite pruebas adicionales también puede ser correcta. Revisa el código y las pruebas antes de autorizar la combinación, y actualiza las pruebas afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller, actualizaste instrucciones, personalizaste una habilidad, creaste un perfil QA y configuraste un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y adáptalas a las necesidades del equipo. Las instrucciones definen estándares, las habilidades describen tareas repetibles, los agentes personalizados establecen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados reales de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para definir un enfoque antes de empezar, **Interactive** para participar en cambios acotados y **Autopilot** para tareas bien delimitadas. Elige un modelo más rápido para cambios rutinarios y otro con más capacidad para tareas complejas.

El contexto importa tanto como la infraestructura. Explicar con claridad *qué* quieres desarrollar, *por qué* y *cómo* cambia de forma significativa el resultado.

## Más opciones para explorar

Ya has cubierto el flujo de trabajo principal. Estas funciones de CLI también pueden resultarte útiles:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otro punto de vista.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar otra tarea.
- `/delegate` para enviar una tarea al agente de GitHub Copilot en la nube.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir usándola. Úsala en código de producción, proyectos personales o esa pequeña aplicación que hace años que quieres crear. Comparte lo que aprendas con el equipo y aprende de sus experiencias. Y, como siempre, consulta la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional de [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
