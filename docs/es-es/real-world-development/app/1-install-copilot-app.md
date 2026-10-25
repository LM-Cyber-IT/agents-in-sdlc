---
title: "Lección 1 - Instalar la aplicación GitHub Copilot"
description: "Instala la aplicación GitHub Copilot, conecta el repositorio creado a partir de la plantilla, familiarízate con el espacio de trabajo y prueba un chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La **[aplicación GitHub Copilot][about-copilot-app]** es una aplicación de escritorio para el desarrollo dirigido por agentes. Se basa en GitHub Copilot CLI y se integra de forma nativa con GitHub, de modo que los repositorios, las ramas y las canalizaciones de integración continua (CI) funcionan sin configuración adicional. Está diseñada para flujos de trabajo en los que diriges varios agentes en paralelo, cada uno en un espacio de trabajo aislado, y automatizas tareas repetitivas. Con Node.js instalado y el proyecto preparado, ahora instalarás la aplicación y conectarás el repositorio.

En esta lección, harás lo siguiente:

- Instalarás la aplicación GitHub Copilot e iniciarás sesión.
- Añadirás el proyecto a la aplicación desde su repositorio de GitHub.
- Te familiarizarás con el espacio de trabajo y el backlog inicial de la plantilla.
- Probarás un chat rápido para conocer la aplicación.

## Escenario

Tu equipo está adoptando agentes de inteligencia artificial (IA) para gestionar un backlog creciente. La aplicación Copilot ofrece un único lugar para dirigir ese trabajo: elegir incidencias, ejecutar agentes, revisar cambios y combinar solicitudes de incorporación de cambios. En esta lección instalarás la aplicación, la conectarás y aprenderás a iniciar una conversación sobre el proyecto.

> [!NOTE]
> Necesitas un plan de Copilot válido: Copilot Free, Copilot Student o un plan de pago (Pro, Pro+, Business o Enterprise). Si usas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva **GitHub Copilot app**.

## Instala y configura la aplicación GitHub Copilot

Hay versiones de la aplicación para Windows, macOS y Linux. Instálala, inicia sesión y añade el repositorio de Tailspin Toys.

1. En un navegador, abre la [página de la aplicación GitHub Copilot][download-app].
2. Descarga la aplicación para tu plataforma e instálala según las instrucciones de la página.
3. Abre la aplicación.
4. Selecciona **Sign in to GitHub** y sigue las instrucciones para autenticarte. Si usas GitHub Enterprise Cloud con residencia de datos, selecciona **Use GitHub Enterprise** e introduce el nombre de host `*.ghe.com` cuando se te solicite. GitHub Enterprise Server no es compatible.
5. Si el proceso de incorporación te pide elegir un repositorio, selecciona el repositorio de Tailspin Toys que creaste a partir de la plantilla. También puedes omitir este paso y añadir el proyecto después.
6. Si se te pide elegir un tema, selecciónalo y termina el proceso de incorporación.
7. Busca el repositorio en **Sessions** en la barra lateral. Si ya aparece, no hace falta que lo añadas otra vez.
8. Si el repositorio aún no está conectado, selecciona **+** junto a **Sessions** para iniciar una sesión nueva y, después, **Add GitHub repository**. Introduce el propietario y el nombre del repositorio anotados en los requisitos previos.
9. Confirma que el repositorio aparece en la barra lateral. En esta disposición, las carpetas del repositorio, como `tailspin-toys`, se agrupan en **Sessions** y las sesiones individuales aparecen debajo de cada repositorio.

## Familiarízate con el espacio de trabajo

Con el proyecto conectado, conoce las áreas de la barra lateral:

- **New**: inicia una sesión de chat con Copilot.
- **Pull requests**: explora y filtra solicitudes de incorporación de cambios, comprueba el estado de CI y revísalas mediante la integración nativa con GitHub.
- **Issues**: explora y filtra incidencias e inicia sesiones desde ellas.
- **Automations**: guarda tareas de agente para ejecutarlas según una programación o bajo demanda. Son útiles para listas de tareas, mantenimiento periódico y otras tareas repetitivas. En este taller se presentan como un siguiente paso, no como otro ejercicio.
- **Customize**: añade servidores MCP, plugins, habilidades y otros componentes a la aplicación. Lo usarás para configurar Playwright MCP.
- **Chats**: inicia conversaciones sencillas para preguntas e ideas que no necesitan rama ni espacio de trabajo. Esta sección aparece después del primer chat. Si no aparece, selecciona **+** junto a **Sessions** para iniciar un chat.
- **Sessions**: agrupa las carpetas de repositorio y las sesiones de agente. Selecciona un repositorio, como `tailspin-toys`, para abrir su vista, o selecciona una sesión debajo para volver a esa conversación. Cada sesión se ejecuta en un espacio de trabajo aislado, así que puedes ejecutar varias sin que sus cambios entren en conflicto. Iniciarás tu primera sesión cuando añadas valoraciones por estrellas.

> [!TIP]
> Si tienes dudas o no sabes si algo es posible, pregúntale a Copilot.

### Localiza el backlog inicial

Explora el backlog que se creó al generar el repositorio desde la plantilla.

1. En **Sessions**, selecciona la carpeta del repositorio de Tailspin Toys, no una sesión individual. Después, selecciona **Issues** en la navegación superior del repositorio.
2. Busca estas incidencias por su título, no por el número:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecciona una incidencia y lee los detalles. Cada incidencia también permite iniciar una sesión de agente. Después de completar un primer cambio rápido, empezarás desde la incidencia de filtrado.

> [!NOTE]
> Las listas de **Issues** y **Pull requests** solo muestran elementos de los repositorios añadidos a la aplicación Copilot. Para ver elementos de otros repositorios, añádelos a la aplicación.

## Prueba un chat rápido

Los chats rápidos permiten hacer preguntas o intercambiar ideas sin crear una rama ni un árbol de trabajo. Son adecuados para consultas breves que no requieren una sesión.

1. Selecciona **+** junto a **Chats** en la barra lateral para abrir un chat. Si **Chats** aún no aparece, selecciona **+** junto a **Sessions** para iniciar el primer chat; después aparecerá **Chats**.
2. Pregunta a la aplicación cómo funcionan sus propias sesiones:

   ```plaintext
   ¿Cómo utiliza la aplicación GitHub Copilot los árboles de trabajo?
   ```

3. Lee la respuesta en la conversación. Verás que cada sesión se ejecuta en su propio árbol de trabajo Git aislado, lo que permite ejecutar varios agentes en paralelo sin conflictos. Puedes continuar la conversación o iniciar otro chat.

## Resumen y siguientes pasos

Has instalado la aplicación GitHub Copilot, conectado el proyecto y explorado el espacio de trabajo. Has aprendido a:

- Instalar la aplicación e iniciar sesión en GitHub.
- Añadir un proyecto desde su repositorio de GitHub.
- Familiarizarte con el espacio de trabajo y localizar el backlog inicial en **Issues**.
- Usar un chat rápido para formular una pregunta breve.

A continuación, [iniciarás tu primera sesión de agente][next-lesson] y mostrarás una valoración por estrellas en las tarjetas de juegos.

## Recursos

- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Introducción a la aplicación GitHub Copilot][getting-started]
- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
