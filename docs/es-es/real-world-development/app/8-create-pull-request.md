---
title: "Lección 8 - Crear y combinar la solicitud de incorporación de cambios (PR) de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la habilidad, el perfil de control de calidad (QA) y las pruebas; después, autoriza Agent Merge para gestionar la solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Ahora revisarás el trabajo y abrirás una solicitud de incorporación de cambios. Combinaste personalmente la solicitud de incorporación de cambios (PR) de valoraciones por estrellas; esta vez permitirás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separarías la funcionalidad, las instrucciones, la actualización de la habilidad y el agente QA en varias PR. Para simplificar el taller, has mantenido el flujo completo de filtrado y calidad en una sesión y una rama, y todo ese trabajo irá a esta PR.

En esta lección, harás lo siguiente:

- Aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- Inspeccionarás la PR completa de la funcionalidad y las pruebas de verificación.
- Autorizarás Agent Merge solo después de revisar el trabajo y confirmarás que la PR se ha combinado.

## Escenario

Durante el flujo de trabajo del filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR y mantener el control del desarrollador sobre la autorización para combinarla.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Cuando lo habilitas, la sesión de la aplicación lee la PR y resuelve lo que la bloquea: corrige comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y vuelve a basar la rama cuando hace falta. La combina en cuanto GitHub lo permite. El proceso se ejecuta en segundo plano, continúa después de reiniciar la aplicación y se desactiva cuando la PR se combina.

Hasta ahora has seleccionado **Merge pull request** personalmente. Agent Merge puede encargarse de esa responsabilidad, pero sigue necesitando tu autorización explícita para editar código y combinar. Revisa las acciones permitidas y el trabajo antes de conceder permiso para combinar.

## Usa Agent Merge para gestionar la PR

Con el código creado y revisado, deja que Agent Merge gestione el proceso de la PR.

1. En el selector de agentes, selecciona **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

Agent Merge empezará a trabajar. Puede:

- Crear la solicitud de incorporación de cambios con un título y una descripción.
- Incluir en la descripción una referencia a la incidencia si iniciaste la sesión desde una.
- Volver a basar la rama o resolver posibles conflictos con la rama de destino.
- Supervisar el proceso de CI para comprobar que se superan todas las verificaciones.
- Supervisar la PR por si otros desarrolladores o la revisión de código de Copilot dejan comentarios y hacer los cambios necesarios para resolverlos.
- Combinar automáticamente la PR cuando todo se haya completado correctamente, si se lo autorizas.

Para que Agent Merge también combine la PR cuando se superen todas las comprobaciones, sigue estos pasos:

5. Revisa la diferencia de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que **Merge pull request** está marcado.
8. Pide explícitamente al agente que combine la PR:

   ```plaintext
   Combina la PR cuando se hayan superado todas las comprobaciones y aprobaciones obligatorias. No eludas las protecciones del repositorio.
   ```

9. Sigue el progreso y resuelve los bloqueos que se indiquen. Antes de continuar, confirma que el estado de la PR es **Merged**; habilitar el permiso o ver que el agente está en ejecución no confirma que la PR se haya combinado.

> [!IMPORTANT]
> Agent Merge no elude las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación, y ahora el proceso de la solicitud de incorporación de cambios. En esta lección:

- Aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- Inspeccionaste la PR completa de la funcionalidad y las pruebas de verificación.
- Autorizaste Agent Merge después de revisar el trabajo y confirmaste que la PR se había combinado.

A continuación, [usarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar otra forma de inspeccionar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
