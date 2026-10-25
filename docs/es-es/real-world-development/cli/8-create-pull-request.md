---
title: "Lección 8 - Crear y combinar la solicitud de incorporación de cambios (PR) de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la habilidad actualizada, el perfil de control de calidad (QA) y las pruebas; después, crea una solicitud de incorporación de cambios (PR) y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Ahora revisarás todo el trabajo y abrirás una solicitud de incorporación de cambios. Combinaste manualmente la solicitud de incorporación de cambios (PR) de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente dividirías la funcionalidad, las instrucciones, la actualización de la habilidad y el agente QA en varias PR. Para simplificar el taller, has mantenido el flujo completo de filtrado y calidad en una conversación y una rama, y lo incluirás en esta PR.

En esta lección, harás lo siguiente:

- Aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- Inspeccionarás el cambio completo de la funcionalidad y las pruebas de verificación.
- Crearás la PR del filtrado.
- Habilitarás Agent Merge solo después de revisar el trabajo y confirmar que la PR se ha combinado.

## Escenario

Durante el flujo de trabajo del filtrado has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar las tareas restantes de la PR y mantener el control del desarrollador sobre la autorización para combinarla.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios. Cuando lo habilitas, Copilot resuelve los bloqueos de la PR: corrige comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y vuelve a basar la rama cuando hace falta. Después, habilita la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora, tú has seleccionado **Merge pull request**. Agent Merge puede encargarse de esa responsabilidad. Revisa el trabajo y confirma que está listo antes de habilitar Agent Merge.

## Usa Agent Merge para gestionar la PR

Con el código preparado, revísalo, crea la PR y deja que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo del agente con `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR y la combinará cuando todo finalice correctamente.
7. Confirma que aparece un mensaje de Copilot parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo indicado y, cuando el repositorio lo permita, completa manualmente la combinación revisada.

## Resumen y siguientes pasos

Has automatizado varias partes del desarrollo: la generación de código, las pruebas y la validación y, ahora, el proceso de la solicitud de incorporación de cambios. En esta lección:

- Aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- Inspeccionaste el cambio completo de la funcionalidad y las pruebas de verificación.
- Creaste la PR del filtrado.
- Habilitaste Agent Merge después de revisar el trabajo y confirmaste que la PR se había combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
