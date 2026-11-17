---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente en la nube de Copilot que se ocupe de tres tareas distintas para mejorar la experiencia y añadir funciones. Aunque trabaja de forma autónoma y asíncrona, es importante supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver las tareas con solicitudes de incorporación de cambios (PR) abiertas, explorar las operaciones realizadas y dirigir una sesión activa.

En este ejercicio:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- dirigirás una sesión activa para solicitar una función adicional.

## Escenario

Después de pedir al agente que cree un modo de alto contraste, el equipo decide añadir también un modo claro. Como ya se están actualizando los estilos y se está añadiendo la funcionalidad de alternancia, dirigirás al agente para que incluya ambos modos.

## Revisar las tareas del agente en la nube

Comprueba el estado de las tareas asignadas al agente en la nube de Copilot.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa las tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas asignadas, como:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea activa y revisa las operaciones que ha realizado Copilot, que pueden incluir:
   - obtener el código del repositorio.
   - crear el entorno de trabajo.
   - configurar servidores Model Context Protocol (MCP).
   - realizar los pasos necesarios para completar la tarea.
4. Observa el panel de PR de la derecha, que permite ver la PR y los archivos modificados.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo y el enfoque del agente.

## Dirigir al agente en la nube

Pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

   ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión unos minutos, hasta que indique que terminó la configuración y empezó a trabajar.
3. En el cuadro **Steer active session while Copilot is working**, escribe este prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Tarea del agente en la nube en la página de agentes con el cuadro de dirección de la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo reconoce y lo incorpora al flujo de trabajo.

## Dejar que Copilot trabaje

Copilot incorporará la nueva solicitud cuando termine el paso que estaba ejecutando. El proceso tardará varios minutos; puedes aprovechar ese tiempo para repasar lo que has aprendido.

## Resumen y siguientes pasos

Has explorado la página de agentes de Copilot, el centro de control para supervisar y dirigir las tareas del agente en la nube. Puedes revisar las tareas activas y completadas, seguir el trabajo y cambiar el alcance de una sesión en curso.

En este ejercicio:

- has explorado el centro de control y la página de agentes para supervisar tareas.
- has redirigido una sesión activa para solicitar una función adicional.

Mientras Copilot termina las funciones de accesibilidad, aprenderás a [iterar sobre las PR que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Siguiente ejercicio: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
