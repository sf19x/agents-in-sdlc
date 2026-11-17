---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezaste con un pequeño cambio en el proyecto, pero las modificaciones mayores requieren un proceso más sólido. GitHub Copilot CLI está diseñado para integrarse en el flujo de trabajo de una organización. En esta lección iniciarás un proceso de desarrollo dirigido por agentes: partirás de una incidencia, planificarás una funcionalidad, revisarás el código, comprobarás su comportamiento y, finalmente, la combinarás en el proyecto.

> [!NOTE]
> Utilizarás la misma conversación y rama durante el flujo de trabajo de esta funcionalidad. Normalmente se usarían ramas o PR distintas para los diferentes cambios, pero en este taller se mantiene todo junto para centrarse en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot desde una incidencia de GitHub.
- definirás los requisitos en modo Plan.
- implementarás la funcionalidad en modo Autopilot.
- revisarás el código.
- validarás manualmente la funcionalidad en un navegador reenviado.

En las siguientes lecciones actualizarás las instrucciones del repositorio, personalizarás la aptitud `quality-checks`, añadirás validación MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y sus visitantes necesitan acotar los juegos por categoría y editor. La incidencia del backlog describe la funcionalidad, pero hay que acordar detalles como la combinación de categorías antes de programar. Utilizarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

La inteligencia artificial (IA) no cambia los fundamentos del desarrollo de software; los hace aún más importantes. Un flujo habitual consiste en:

1. Partir de una incidencia que describa el trabajo.
2. Planificar lo que se debe crear.
3. Desarrollar y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código cuando se haya revisado y se hayan superado las comprobaciones de integración continua.

> [!NOTE]
> Los detalles varían según el equipo y la organización, pero la mayoría de los procesos siguen alguna variante de este flujo.

Este enfoque ayuda a garantizar que el código generado por IA cumpla los requisitos y pase por el mismo proceso de validación que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla el grado de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar de modo:

- **Interactive**: trabajas junto con el agente. Propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea un plan y no puede editar archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma, escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en Plan, revisa el plan y, después, úsalo para implementar los cambios en Autopilot.

## Iniciar desde la incidencia

Antes de comenzar el filtrado, vuelve a tu Codespace y prepara el repositorio y el terminal.

1. Vuelve a tu Codespace. Si está detenido, reinícialo.
2. Confirma que la PR de valoraciones por estrellas está combinada.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de dirección para seleccionar **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

El prompt comenzará con `#7` (o un número similar). El carácter `#` permite incluir en el contexto una incidencia o PR de GitHub.

## Planificar el filtrado

La planificación permite definir el enfoque antes de asignar una tarea a Copilot. Para una tarea compleja, dedica tiempo a planificar.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar a Plan. Confirma que el indicador debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia, escribe:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot explorará el proyecto y determinará el enfoque. Puede preguntarte cómo debe funcionar el filtrado; responde según tus preferencias.

3. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
4. Desplázate por el plan y pide a Copilot que revise cualquier parte que no refleje tus decisiones.

## Aprobar Autopilot

Cuando hayas revisado el plan, autoriza la implementación.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Confirma que el indicador debajo del prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación inmediatamente, así que revisa primero el plan. Si Copilot informa de dependencias ausentes o de un conflicto de puerto, resuelve el problema antes de dar las comprobaciones por completadas.

## Revisar y verificar la implementación

Revisa el código generado antes de combinarlo, como harías con cualquier otro cambio.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en modo Interactive. Confirma que el indicador no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones tomadas durante la planificación.
4. Pulsa <kbd>Esc</kbd> para salir de la vista del diff.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que resuelva cualquier fallo.

## Explorar la nueva funcionalidad

Inicia la aplicación y abre el sitio a través del puerto reenviado de Codespaces para comprobar que funciona.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría, el filtrado por editor y las combinaciones acordadas en el plan.
4. Confirma que el restablecimiento y el estado sin resultados coinciden con la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y siguientes pasos

Has utilizado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- has iniciado una conversación de Copilot desde una incidencia de GitHub.
- has definido los requisitos en modo Plan.
- has implementado la funcionalidad en modo Autopilot.
- has revisado el código.
- has validado manualmente la funcionalidad en un navegador reenviado.

A continuación, profundizarás en cómo se genera el código mediante el [uso de instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
