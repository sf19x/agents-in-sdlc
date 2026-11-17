---
title: "Lección 10 - Conclusiones y siguientes pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI durante un flujo de trabajo continuo en Tailspin Toys. En concreto:

- has preparado un Codespace, instalado Copilot CLI, explorado el proyecto y localizado la incidencia de filtrado.
- has añadido las valoraciones por estrellas, revisado el resultado en un navegador reenviado y combinado manualmente tu primera solicitud de incorporación de cambios (PR).
- has partido de la incidencia de filtrado, definido el enfoque en modo Plan, implementado con Autopilot y revisado en modo Interactive.
- has guiado al agente con instrucciones personalizadas, personalizado la aptitud `quality-checks` y utilizado esta para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- has añadido el servidor Model Context Protocol (MCP) de Playwright y lo has utilizado para explorar el filtrado en un navegador.
- has creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la aptitud y las pruebas del navegador.
- has revisado el cambio completo de filtrado y autorizado Agent Merge para combinar su PR.
- has explorado comandos slash para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Lo que has entregado

El taller incluye dos hitos de PR, cada uno en una rama basada en `main` actualizado:

1. **Valoraciones por estrellas:** muestra `starRating` y un estado explícito para los juegos sin valorar en las tarjetas.
2. **Filtrado y flujo de calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la apertura de su PR, has usado la misma conversación y rama. Agrupamos ese trabajo en una PR para agilizar el taller.

## Distintos tipos de verificación

Has comprobado el código de varias maneras: con pruebas automatizadas, mediante tu propia inspección en el navegador y con la exploración del navegador de Copilot a través de MCP. La aptitud `quality-checks` ejecutó las pruebas unitarias, lint y comprobaciones de tipos, y presentó los resultados con el nuevo formato. QA reunió esos resultados con una revisión de los requisitos y la cobertura antes de la PR.

Las pruebas nuevas deben cubrir carencias reales; si una ejecución de QA determina que no hacen falta, puede ser correcto. Revisa el código y las pruebas antes de autorizar la combinación, y actualiza las pruebas afectadas después de hacer cambios.

## Buenas prácticas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller actualizaste instrucciones, personalizaste una aptitud, creaste un perfil de QA y configuraste un servidor MCP. Reutiliza estas personalizaciones entre conversaciones y ajústalas a las necesidades del equipo. Las instrucciones establecen estándares, las aptitudes describen tareas repetibles, los agentes personalizados definen roles especialistas y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para definir el enfoque, **Interactive** para mantenerte al tanto de los cambios acotados y **Autopilot** para tareas bien definidas. Elige un modelo más rápido para cambios rutinarios y otro más capaz para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Explicar con claridad *qué* quieres crear, *por qué* y *cómo* influye de forma significativa en el resultado.

## Más opciones para explorar

Has completado el flujo principal. También puedes probar estas funciones de la CLI:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar una tarea distinta.
- `/delegate` para enviar una tarea al agente en la nube de Copilot.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola: para código de producción, proyectos personales o una aplicación que tengas pendiente desde hace tiempo. Comparte lo que aprendas con el equipo y aprovecha lo que aprendan los demás. Consulta también la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora los talleres de [VS Code][vscode], la [aplicación GitHub Copilot][app] o el [agente en la nube de Copilot][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
