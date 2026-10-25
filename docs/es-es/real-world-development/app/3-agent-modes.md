---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: usa Plan para acordar un enfoque, Autopilot para implementar el filtrado desde una incidencia e Interactive para revisar y verificar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Empezaste por añadir una pequeña funcionalidad al proyecto, pero los cambios más amplios requieren un proceso más sólido. La aplicación GitHub Copilot se adapta al flujo de trabajo existente de una organización para ayudar a desarrollar lo adecuado de la forma correcta. En esta y las lecciones siguientes seguirás un proceso de desarrollo habitual con agentes: partirás de una incidencia, crearás una funcionalidad, comprobarás el código y el comportamiento y, finalmente, la combinarás en el proyecto.

> [!NOTE]
> Usarás la misma sesión durante todo el flujo de trabajo de la funcionalidad. Normalmente se usarían sesiones o solicitudes de incorporación de cambios (PR) distintas para diferentes tipos de archivos, pero aquí simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección, harás lo siguiente:

- Iniciarás una sesión de agente nueva desde una incidencia de GitHub.
- Definirás los requisitos en modo **Plan**.
- Implementarás la funcionalidad con el modo **Autopilot**.
- Revisarás el código.
- Validarás manualmente la funcionalidad en un lienzo de navegador.

En las lecciones siguientes actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás validación MCP, crearás un agente de control de calidad (QA) y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys crece y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero antes de programar hay que acordar algunos detalles, como la combinación de categorías. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con inteligencia artificial (IA) al flujo de desarrollo no cambia los principios fundamentales. De hecho, los hace aún más importantes. Un flujo habitual se parece a este:

1. Abrir una incidencia que detalle el trabajo necesario.
2. Crear un plan de desarrollo.
3. Crear y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código cuando se haya revisado y se hayan superado las comprobaciones de integración continua.

> [!NOTE]
> El proceso concreto varía según el equipo y la organización, pero suele seguir una secuencia parecida.

Este enfoque ayuda a garantizar que el código generado por IA cumpla los requisitos y pase por el mismo proceso de validación que el código escrito manualmente.

## Modos de sesión

El **modo de sesión** controla el grado de autonomía del agente. Puedes elegirlo en el menú desplegable situado debajo del campo del prompt y cambiarlo en cualquier momento:

- **Interactive**: trabajas con el agente. Propone cambios y espera tu intervención antes de continuar.
- **Plan**: el agente crea un plan y espera a que lo revises y apruebes antes de ejecutarlo.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera sin esperar instrucciones.

Empieza en modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Inicia una sesión desde la incidencia

Antes de empezar, confirma que se ha combinado la PR de valoraciones por estrellas y que tu rama local `main` está actualizada.

1. En **Sessions**, selecciona la carpeta del repositorio de Tailspin Toys y, después, **Issues** en la navegación superior del repositorio. Abre la incidencia **Allow users to filter games by category and publisher**. Es una incidencia existente creada desde la plantilla; no hace falta crear otra.
2. Selecciona **New session** y elige un **new working tree** basado en `main` actualizado.

   ![Vista de la incidencia en la aplicación GitHub Copilot con una flecha que señala el botón New session](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia está adjunta a la sesión y selecciona **Plan** en el selector de modo.

> [!NOTE]
> Si no se encuentra Node.js o npm, pide al agente que ejecute `node --version` y `npm --version`. Si funcionan en el terminal, pero no en la sesión, revisa la configuración de `PATH` o del gestor de versiones de la aplicación y ciérrala y vuelve a abrirla por completo. Reintenta la operación y las comprobaciones fallidas. No reinstales npm solo porque el agente no lo encuentre.

## Planifica la funcionalidad de filtrado

Planificar permite revisar el enfoque antes de que Copilot escriba código. Como iniciaste la sesión desde la incidencia, Copilot ya tiene la solicitud de la funcionalidad como contexto. Envía:

```plaintext
Implementa esta funcionalidad.
```

Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que incluye filtrado por categoría y editorial, controles accesibles, cambios en el acceso a datos y pruebas. Aclara los comportamientos que no estén definidos, como la combinación de varias categorías o qué ocurre cuando ningún juego coincide.

El plan debe incluir lint, pruebas unitarias, pruebas de extremo a extremo (E2E) y comprobaciones de tipos con las herramientas existentes del proyecto. Céntralo en implementar y probar el filtrado; crearás la PR al completar el flujo de calidad. Pide cambios en el plan antes de aprobarlo y conserva la URL de la incidencia y las aclaraciones acordadas para la validación posterior.

## Aprueba Autopilot explícitamente

Cuando estés conforme con el plan, selecciona **Approve and implement this plan** o la opción equivalente de tu versión. No des por hecho que la aprobación cambia el modo: confirma que el indicador muestra **Autopilot**.

Copilot comenzará a implementar la funcionalidad y a seguir el plan, generar código y ejecutar pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa primero el plan. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar las comprobaciones por completadas. Detén solo los servidores que hayas iniciado.

## Revisa y verifica la implementación

Revisa el código y ejecuta el sitio para comprobar que todo funciona antes de combinar los cambios.

1. Abre **Changes** e inspecciona la implementación del filtrado y las pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de categorías y editoriales. Comprueba que los cambios respetan las instrucciones del repositorio.
3. Revisa los resultados de lint, las pruebas unitarias, las pruebas E2E y la comprobación de tipos. Una comprobación omitida no cuenta como superada.
4. Resuelve los fallos y vuelve a ejecutar las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright compila y sirve una vista previa y puede reutilizar un servidor local; confirma que el servidor probado pertenece a este árbol de trabajo y no a una lección anterior.

## Explora la nueva funcionalidad

Pide a Copilot que inicie el sitio y lo abra en un lienzo de navegador.

1. Envía este prompt:

   ```plaintext
   Inicia la aplicación y ábrela en el lienzo del navegador.
   ```

2. Espera a que se inicie la aplicación y se abra el navegador dentro de la aplicación Copilot.
3. Selecciona una categoría y confirma que los resultados corresponden a ella. Borra el filtro, selecciona una editorial y comprueba que los resultados corresponden a esa editorial.
4. Combina los filtros de categoría y editorial y compara los resultados con la incidencia y el plan aprobado. Si se admiten varias categorías, comprueba el comportamiento acordado.
5. Prueba una combinación sin resultados y comprueba el estado vacío. Borra o restablece los filtros y confirma que vuelve a aparecer la lista completa.
6. Cuando termines, pide a Copilot que detenga el servidor de desarrollo que inició para esta sesión:

   ```plaintext
   Detén el servidor de desarrollo y cierra el lienzo del navegador.
   ```

## Conserva esta sesión para las lecciones siguientes

Todavía no crees ni combines la PR del filtrado. Conserva esta sesión, el árbol de trabajo y la rama para las lecciones 4 a 8. A continuación aplicarás instrucciones personalizadas al código, añadirás las mejoras de habilidades y QA y crearás y combinarás la PR conjunta en la lección 8. Puedes dejar la sesión inactiva después de detener el servidor y volver a ella en la siguiente lección.

## Resumen y siguientes pasos

Has usado distintos modos de agente para desarrollar y revisar una funcionalidad. En esta lección:

- Iniciaste una sesión de agente nueva desde una incidencia de GitHub.
- Definiste los requisitos en modo **Plan**.
- Implementaste la funcionalidad con el modo **Autopilot**.
- Revisaste el código.
- Validaste manualmente la funcionalidad en un lienzo de navegador.

A continuación, profundizarás en la generación de código y en cómo seguir las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
