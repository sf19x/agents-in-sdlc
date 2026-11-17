---
title: "Lección 9 - Comandos slash de GitHub Copilot CLI"
description: "Explora comandos slash para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar trabajo opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como otras herramientas CLI, GitHub Copilot CLI incluye comandos slash para interactuar con ella. Estos comandos ofrecen funciones avanzadas, información interna y opciones de configuración. Ya has utilizado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Ahora explorarás otros útiles.

En esta lección:

- usarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- usarás `/model` para explorar los modelos disponibles.
- aprenderás cómo `/share` puede exportar o compartir una sesión.
- explorarás comandos opcionales para trabajar en paralelo, usar worktrees y delegar tareas al agente en la nube.

## Escenario

Has terminado el flujo de trabajo principal de la CLI. Ahora explorarás otras capacidades: gestionar el contexto, cambiar de modelo, compartir sesiones y delegar trabajo opcionalmente al [agente en la nube de Copilot][about-cloud-agent].

## Explorar el contexto de Copilot CLI

En tareas grandes o complejas puedes acercarte al límite de contexto del modelo. Copilot CLI compacta la conversación automáticamente cuando hace falta, y puedes inspeccionar o compactar el contexto con comandos slash.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si todavía no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y cómo se divide el contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio disponible.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Si la conversación ya es pequeña, quizá no haya un cambio importante.

> [!NOTE]
> Copilot CLI compacta el contexto automáticamente a medida que se llena la ventana. Usa `/compact` para elegir cuándo hacerlo. Usa `/clear` o `/new` si cambias a una tarea no relacionada y quieres iniciar una conversación nueva.

## Elegir un modelo

Los modelos tienen distintas ventajas y cada desarrollador puede tener sus preferencias. Copilot CLI permite enumerar y seleccionar el modelo que quieras.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información de uso.
3. Conserva el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Compartir una sesión

El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` disponibles en tu versión.
2. Si quieres compartir la sesión, elige el destino adecuado, como `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de una conversación que el equipo no tenga previsto compartir.

## Opcional: escalar o delegar

El flujo principal del taller ha terminado. Copilot CLI también ofrece comandos para tareas mayores:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un worktree de Git aislado para una tarea distinta.
- `/delegate` puede enviar una tarea al agente en la nube de Copilot, que trabaja de forma asíncrona y puede abrir una PR.

Estos comandos son opcionales porque pueden crear worktrees adicionales o trabajo remoto. Antes de probarlos, inicia una tarea nueva y acotada, y revisa el resultado con tu flujo habitual. Para profundizar en el trabajo asíncrono con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y siguientes pasos

Has usado comandos slash de Copilot CLI para configurarlo, compartir sesiones y consultar información interna. En esta lección:

- has usado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto.
- has usado `/model` para explorar los modelos disponibles.
- has aprendido cómo `/share` puede exportar o compartir una sesión.
- has explorado comandos opcionales para trabajar en paralelo, usar worktrees y delegar al agente en la nube.

Hay más comandos slash disponibles. Para terminar el recorrido, [repasarás lo aprendido][next-lesson] y algunos pasos para continuar.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente en la nube de Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
