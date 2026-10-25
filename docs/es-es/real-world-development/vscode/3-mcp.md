---
title: "Ejercicio 3 - Probar la funcionalidad con Playwright y Model Context Protocol (MCP)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Modo agente][previous-lesson] · [Siguiente ejercicio: Agentes personalizados →][next-lesson]

Has creado la funcionalidad de filtrado con el modo agente. Antes de abrir una solicitud de incorporación de cambios (PR), comprobarás que funciona en el navegador. Configurarás el servidor Playwright de **Model Context Protocol (MCP)** para que Copilot controle un navegador real, pruebe el filtrado y, después, publique la rama y abra la PR.

En este ejercicio, harás lo siguiente:

- Aprenderás qué es Model Context Protocol (MCP) y cómo amplía las herramientas de Copilot.
- Añadirás el servidor Playwright MCP al espacio de trabajo.
- Pedirás a Copilot que pruebe manualmente la funcionalidad en un navegador.
- Publicarás la rama y abrirás una solicitud de incorporación de cambios.

## ¿Qué es Model Context Protocol (MCP)?

El modo agente es más útil cuando puede acceder a herramientas fuera del editor. Model Context Protocol (MCP) es un estándar que permite a Copilot comunicarse con herramientas y servicios externos.

![Diagrama de la interacción del modo agente con el contexto, el modelo de lenguaje y las herramientas, incluidas las proporcionadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)][mcp-blog-post] permite a los agentes de inteligencia artificial (IA) comunicarse con herramientas y servicios externos, acceder a información actualizada y realizar acciones en tu nombre. Un servidor MCP hace de puente entre el agente y las herramientas o servicios, como las interfaces de programación de aplicaciones (API) y los paquetes locales.

Algunos servidores conocidos son:

- **[GitHub MCP Server][github-mcp-server]**: proporciona interfaces de programación de aplicaciones (API) para gestionar repositorios, incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server][playwright-mcp-server]**: automatiza el navegador para navegar por páginas, rellenar formularios y seleccionar botones.

GitHub aloja un [registro MCP][mcp-registry] para facilitar el descubrimiento de servidores y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia. Revisa el código fuente, comprueba quién lo publica y considera las implicaciones de seguridad. Usa solo servidores de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en el espacio de trabajo de VS Code. El proyecto ya incluye el servidor remoto de GitHub, que permitió a Copilot leer el backlog en el ejercicio anterior.

1. Abre `.vscode/mcp.json` en tu Codespace.
2. Comprueba que incluye esta configuración:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

VS Code se conecta al servidor remoto de GitHub MCP mediante el protocolo de transferencia de hipertexto (HTTP) y te pide autenticarte la primera vez que Copilot usa sus herramientas.

## Añade el servidor Playwright MCP

Añade un segundo servidor para que Copilot pueda controlar un navegador.

1. En `.vscode/mcp.json`, añade `playwright` junto a `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Guarda el archivo.

`playwright` es un servidor local que VS Code inicia con `npx @playwright/mcp@latest`. La opción `--headless` permite ejecutar el navegador sin una ventana visible, algo necesario en Codespaces.

> [!NOTE]
> Tailspin Toys ya usa Playwright para las pruebas de extremo a extremo, así que normalmente el navegador ya está instalado. Si Copilot indica que falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Inicia el servidor y confía en él

VS Code inicia los servidores MCP cuando se necesitan. La primera vez, te pedirá que confirmes si confías en el servidor.

1. En `.vscode/mcp.json`, selecciona la acción **Start** que aparece encima de `playwright` para iniciarlo y comprobar la configuración.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en él.

Este paso es opcional: si no inicias el servidor manualmente, VS Code lo iniciará cuando Copilot necesite sus herramientas y te pedirá que confirmes si confías en él.

## Prueba la funcionalidad de filtrado

El agente puede abrir una página, aplicar filtros y comunicar los resultados. Playwright MCP usa el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de capturas de pantalla. El agente interpreta los elementos estructurados y etiquetados, como los botones, enlaces y elementos de lista. Así, una comprobación funcional también sirve como revisión básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, envía este prompt:

```text
Usa el servidor Playwright MCP para abrir un navegador en la aplicación en ejecución en http://localhost:4321 y verificar la nueva funcionalidad de filtrado de juegos:

1. Ve a la página de juegos e indica cuántos juegos aparecen.
2. Aplica un filtro de categoría y confirma que la lista solo muestra juegos de esa categoría.
3. Bórralo y aplica un filtro de editorial; confirma que la lista solo muestra juegos de esa editorial.
4. Combina un filtro de categoría y otro de editorial y confirma que los resultados respetan ambos.

Describe lo que observas en cada paso e indica cualquier comportamiento inesperado.
```

Copilot abrirá el navegador mediante Playwright MCP y seguirá los pasos. Compara el informe con los criterios de aceptación de la incidencia. Si detectas un problema, pide más información o solicita que corrija el código antes de abrir la PR.

> [!NOTE]
> La aplicación debe estar disponible en `http://localhost:4321`. Si detuviste el servidor, vuelve a iniciarlo antes de enviar el prompt. La primera vez que se usa Playwright MCP, puede que haya que descargar un navegador; si falta, pide a Copilot que ejecute `npx playwright install chromium`.

## Publica la rama y crea una solicitud de incorporación de cambios

Una vez comprobada la funcionalidad, publica la rama `filtering-vscode` para que el equipo pueda revisar los cambios.

1. Abre el panel **Source Control** del Codespace y revisa los cambios de Copilot.
2. Prepara los cambios con el botón **+**.
3. Usa el botón **Sparkle** para generar un mensaje de confirmación.

   ![Panel Source Control con los cambios realizados por Copilot](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la solicitud de incorporación de cambios

Pide a Copilot que cree la PR y la asocie a la incidencia pertinente.

1. Abre Copilot Chat y selecciona **New Chat**.
2. En el selector de agentes, confirma que está seleccionado **Agent** para que Copilot pueda usar las herramientas de GitHub.
3. Envía este prompt:

   ```plaintext
   Busca en el repositorio la incidencia relacionada con el filtrado por categoría y editorial. Crea una solicitud de incorporación de cambios para la rama actual y asóciala a la incidencia correcta.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir que Copilot recopile información y realice acciones. Si te pide que te autentiques con GitHub, sigue las instrucciones.
5. Comprueba que Copilot encuentra la incidencia correcta y crea la PR.
6. Selecciona el enlace de la PR para revisarla, pero todavía no la combines.

## Resumen y siguientes pasos

En este ejercicio:

- Aprendiste qué es Model Context Protocol (MCP) y cómo amplía las herramientas de Copilot.
- Añadiste el servidor Playwright MCP al espacio de trabajo.
- Lo usaste para probar manualmente la funcionalidad en un navegador antes de entregarla.
- Publicaste la rama y abriste una solicitud de incorporación de cambios.

Has usado MCP para probar una funcionalidad, pero hay otras formas de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué todo el mundo habla de ello?][mcp-blog-post]
- [Servidor Microsoft Playwright MCP][playwright-mcp-server]
- [Servidor GitHub MCP][github-mcp-server]
- [Registro de servidores MCP][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
