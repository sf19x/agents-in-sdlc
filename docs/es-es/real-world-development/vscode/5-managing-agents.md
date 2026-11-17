---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] |
|:--|

En el modo de agente, GitHub Copilot trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como trabaja en el equipo y en tiempo real, Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición. Puedes revisar cada diff, aceptar o rechazar cambios y dirigir la conversación con prompts de seguimiento.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios de archivo propuestos.
- pedirás al agente que añada un control para el modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un modo claro. En lugar de empezar una conversación nueva, ampliarás el alcance de la sesión existente.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra los pasos del agente: archivos que explora, comandos que ejecuta y cambios que propone. Cada edición aparece en línea con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver las llamadas a herramientas, lecturas y ediciones de archivos y comandos del terminal.
3. Cuando proponga una edición:
   - selecciona **Keep** para aceptar el cambio.
   - selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. Abre **Files changed** en la parte superior del chat para ver todos los archivos modificados en la sesión.

> [!NOTE]
> Si una llamada a una herramienta necesita permiso, por ejemplo, para ejecutar un comando del terminal, el agente se detendrá y mostrará **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje final que resuma los cambios.

## Dirigir al agente para añadir el modo claro

Como continúas en la misma conversación, el agente conserva el contexto de su trabajo: los componentes que modificó, el enfoque de estilos y el uso de almacenamiento local.

1. Envía este prompt de seguimiento desde Copilot Chat:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. El agente revisará los cambios existentes y propondrá actualizaciones para el modo claro.
3. Revisa cada cambio nuevo y selecciona **Keep** o **Undo** en cada diff.
4. Si el agente toma una dirección que no quieres, por ejemplo, si intenta reorganizar los controles, detenlo con **Stop**, junto al campo de chat, y envía un mensaje para aclararlo.

> [!TIP]
> Dirigir una sesión activa es más rápido que empezar de nuevo. Usa prompts de seguimiento para solicitar ajustes, corregir el rumbo o ampliar el alcance; por ejemplo, puedes pedir que simplifique el CSS o que integre el modo claro en el panel existente.

## Revisar los cambios

Antes de confirmar el trabajo, revisa todos los archivos que tocó el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados; debería incluir componentes Astro, estilos y las pruebas relacionadas.
3. Abre algunos archivos y revisa sus diffs. Confirma que se aplican los patrones de accesibilidad del agente personalizado: atributos de aplicaciones de Internet enriquecidas y accesibles (ARIA), navegación por teclado, lenguaje de marcado de hipertexto (HTML) semántico y persistencia en almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios en el panel Source Control. Los publicarás en el [siguiente ejercicio][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo de agente de Copilot Chat para supervisar y dirigir una sesión activa: has revisado cada edición, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En este ejercicio:

- has supervisado el trabajo de un agente en tiempo real.
- has aceptado y rechazado cambios de archivo individuales.
- has dirigido una sesión activa con prompts de seguimiento.

A continuación, publicarás las funciones de accesibilidad como una solicitud de incorporación de cambios y revisarás los cambios del agente local personalizado en este taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Siguiente ejercicio: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
