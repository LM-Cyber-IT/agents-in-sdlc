---
title: "Lección 7 - Crear y usar un agente de control de calidad (QA)"
description: "Crea un agente de control de calidad (QA) que combine los requisitos de la incidencia, la habilidad quality-checks y el servidor Playwright de Model Context Protocol (MCP)."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar la experiencia de filtrado en un navegador. Ahora reunirás estas capacidades en un agente personalizado con un proceso de control de calidad (QA) definido.

En esta lección, harás lo siguiente:

- Explorarás cómo combina un agente personalizado las instrucciones, las habilidades y las herramientas MCP.
- Crearás e inspeccionarás un perfil reutilizable de control de calidad.
- Seleccionarás el agente QA y revisarás sus conclusiones según la incidencia de filtrado.

## Escenario

Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador antes de abrir una solicitud de incorporación de cambios (PR). Un agente personalizado puede coordinar el proceso de QA y generar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil Markdown. El perfil describe su propósito, sus instrucciones y las herramientas disponibles. En este taller definirás el rol de control de calidad (QA) en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones tienen funciones distintas: las instrucciones del repositorio describen los estándares del equipo; la habilidad `quality-checks` agrupa comprobaciones repetibles; Playwright MCP proporciona herramientas de navegador; y el perfil QA indica a Copilot cómo usar esas capacidades para evaluar los requisitos y comunicar los resultados. El perfil no las sustituye ni requiere otra conversación.

## Crea el perfil QA

Antes de abrir la PR de la funcionalidad, pide a Copilot que cree un perfil QA reutilizable. El perfil definirá tanto las comprobaciones como los límites que debe respetar.

1. Vuelve a tu Codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Crea un agente personalizado llamado QA en .github/agents/qa.agent.md. Debe comprobar las funcionalidades según sus incidencias y los requisitos acordados, seguir las instrucciones del repositorio, ejecutar la habilidad quality-checks, usar Playwright MCP para verificar el comportamiento y añadir pruebas si falta cobertura.

   Debe indicar cada requisito como pass, fail o blocked y aportar pruebas que respalden el resultado. Debe preguntar antes de cambiar el código de implementación y no debe confirmar cambios ni abrir solicitudes de incorporación de cambios.

   Por ahora, crea solo el perfil para que pueda revisarlo.
   ```

## Inspecciona el perfil

Antes de usar el agente nuevo, revisa su perfil para confirmar que Copilot ha recogido el flujo de trabajo y los límites previstos. Así evitarás que un agente incompleto o demasiado amplio cambie la funcionalidad cuando solo quieres verificarla.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, aunque permite mostrar un nombre claro.
3. Lee las instrucciones del perfil y confirma que QA parte de los requisitos, sigue las instrucciones del repositorio, ejecuta `quality-checks` y usa Playwright MCP.
4. Confirma que QA aporta pruebas, pregunta antes de cambiar el código de implementación y no confirma cambios ni abre solicitudes de incorporación de cambios.
5. Si falta alguna responsabilidad o límite, pide al agente predeterminado que revise el perfil antes de continuar.

## Ejecuta QA con la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Reanuda la conversación de filtrado y selecciona QA para que pueda usar la incidencia y las decisiones de planificación que ya están en contexto.

1. Activa el agente con este prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, es posible que aún no aparezca en la lista. El comando anterior lo activará.

2. Pide al agente QA que revise la funcionalidad:

   ```plaintext
   Revisa la funcionalidad de filtrado según la incidencia y las decisiones de nuestro plan. ¿Está lista para una PR?
   ```

3. Deja que el agente QA realice la revisión.
4. Lee el informe cuando termine.

## Resumen y siguientes pasos

Has añadido un rol especializado reutilizable y has revisado su trabajo. En esta lección:

- Exploraste cómo combina un agente personalizado las instrucciones, las habilidades y las herramientas MCP.
- Creaste e inspeccionaste un perfil reutilizable de control de calidad.
- Seleccionaste el agente QA y revisaste sus conclusiones según la incidencia de filtrado.

Ya tienes la implementación, la actualización de la habilidad, el perfil QA, las pruebas y el informe de verificación listos para revisar. A continuación, [reunirás estos cambios en una PR de funcionalidad y usarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
