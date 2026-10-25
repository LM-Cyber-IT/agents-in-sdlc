---
title: "Lección 6 - Validar la funcionalidad con Playwright y Model Context Protocol (MCP)"
description: "Configura Playwright MCP en Copilot CLI si hace falta y úsalo para explorar en un navegador la funcionalidad de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos comentado, desarrollar no consiste solo en escribir código. También hay que trabajar con datos y servicios externos, y permitir que Copilot use automatizaciones adicionales. Para ello sirven los servidores de Model Context Protocol (MCP): amplían las herramientas y los servicios disponibles en la CLI.

En esta lección, harás lo siguiente:

- Aprenderás qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI.
- Añadirás el servidor Playwright MCP si aún no está disponible.
- Pedirás al agente que controle un navegador y explore la funcionalidad de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de inteligencia artificial (IA) se comuniquen con herramientas y servicios externos en tiempo real y realicen acciones en tu nombre.

Los agentes acceden a estas herramientas y recursos mediante un servidor MCP, que actúa como puente con los servicios externos. Cada servidor ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos son:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a interfaces de programación de aplicaciones (API) para gestionar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: proporciona capacidades de automatización del navegador mediante Playwright.

Hay muchos otros servidores MCP. GitHub aloja un [registro de MCP][mcp-registry] para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa el código fuente, comprueba quién lo publica y considera las implicaciones de seguridad.

## Añade el servidor Playwright MCP

Si Playwright MCP aún no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría un usuario.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa en la sección siguiente.

1. Vuelve a tu Codespace.
2. En Copilot CLI, abre el cuadro de diálogo para añadir un servidor MCP con este comando:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo de servidor **STDIO** con <kbd>Enter</kbd> y vuelve a pulsar <kbd>Tab</kbd>.
5. Pega este comando en el cuadro **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el nuevo servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo MCP.

## Pide a Copilot que explore la funcionalidad con Playwright

Ya has comprobado manualmente que la funcionalidad se comporta como esperabas. Ahora pedirás al agente que haga lo mismo con el servidor Playwright MCP.

1. Usa este prompt para pedir a Copilot que valide la funcionalidad con Playwright MCP:

   ```plaintext
   Inicia la aplicación y usa Playwright MCP para comprobar el filtrado según la incidencia y nuestro plan. Dime qué funciona y qué no, sin hacer cambios. Cuando termines, detén el servidor que has iniciado.
   ```

> [!NOTE]
> No hace falta indicar expresamente a Copilot que use el servidor MCP, porque normalmente lo detectará. Aun así, especificarlo puede ayudar a que la comprobación sea más coherente.

2. Observa los pasos que realiza Copilot en el navegador para comprobar la funcionalidad.
3. Lee el informe y confirma que todo se comporta como esperabas.

Copilot iniciará el servidor, usará Playwright para interactuar con el sitio, detendrá el servidor y te proporcionará un informe.

## Resumen y siguientes pasos

Has usado el servidor Playwright MCP para explorar la funcionalidad en un navegador real desde Copilot CLI. En concreto:

- Aprendiste qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI.
- Añadiste el servidor Playwright MCP si aún no estaba disponible.
- Pediste al agente que controlara un navegador para explorar la funcionalidad de filtrado.

A continuación, [crearás un agente personalizado de control de calidad (QA)][next-lesson] que combinará la habilidad y las herramientas del navegador en un perfil especializado.

## Recursos

- [¿Qué es MCP y por qué todo el mundo habla de ello?][mcp-blog-post]
- [Servidor Microsoft Playwright MCP][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
