---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la aptitud, el perfil de QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las instrucciones actualizadas, la aptitud, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Ahora los revisarás juntos y abrirás una solicitud de incorporación de cambios (PR). Combinaste tú mismo la PR de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las instrucciones, la actualización de la aptitud y el agente de QA en varias PR. Para agilizar el taller, has mantenido el flujo completo de filtrado y calidad en una conversación y una rama.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionarás todo el cambio de la funcionalidad y las pruebas de verificación.
- crearás la PR de filtrado.
- activarás Agent Merge solo después de revisar los cambios y confirmarás que la PR se ha combinado.

## Escenario

Durante el flujo de filtrado has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener el control de autorización de la combinación en manos del desarrollador.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios. Cuando se habilita, Copilot resuelve los bloqueos de la PR, como las comprobaciones de integración continua (CI) que fallan, los comentarios de revisión y las reorganizaciones de la rama. Después, habilita la combinación automática de GitHub cuando la configuración del repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esa tarea. Revisa el trabajo y confirma que está listo antes de activarlo.

## Usar Agent Merge para gestionar la PR

Con todo el código creado, revisa los cambios, crea la PR y deja que Agent Merge se encargue del resto.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo de agentes con `/agent`.
3. Selecciona **Default** y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR y la combinará cuando todo termine correctamente.
7. Confirma que Copilot muestra un mensaje similar a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, revisa el motivo y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del desarrollo: la generación de código, las pruebas y la validación, y ahora el proceso de la PR. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- has inspeccionado el cambio completo de la funcionalidad y las pruebas de verificación.
- has creado la PR de filtrado.
- has activado Agent Merge después de revisar el trabajo y confirmado que la PR se ha combinado.

A continuación, [explorarás otros comandos slash útiles de Copilot CLI][next-lesson] para gestionar contexto, modelos y uso compartido, además de la delegación opcional al agente en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
