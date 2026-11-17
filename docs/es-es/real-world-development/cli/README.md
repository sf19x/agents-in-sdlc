---
slug: es-es/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Desarrolla, verifica y entrega dos cambios de Tailspin Toys mientras exploras los modos, las personalizaciones, las herramientas MCP y la automatización de solicitudes de incorporación de cambios de Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** incorpora GitHub Copilot al terminal como asistente de programación con agentes. Explora bases de código, genera código, ejecuta comandos y se conecta a herramientas externas desde la línea de comandos, para que puedas mantener el ritmo sin cambiar a un editor gráfico.

El taller sigue un flujo de trabajo continuo en Tailspin Toys:

1. Prepara el proyecto en GitHub Codespaces, instala Copilot CLI y familiarízate con él.
2. Haz un cambio acotado para mostrar las valoraciones, revísalo en el navegador y combina manualmente tu primera solicitud de incorporación de cambios (PR).
3. Empieza con la incidencia de filtrado, define el enfoque en el modo Plan, implementa en el modo Autopilot y revisa en el modo Interactive.
4. Actualiza las instrucciones del repositorio y aplícalas al filtrado.
5. Personaliza la aptitud `quality-checks` existente y úsala para ejecutar las comprobaciones del proyecto.
6. Añade el servidor Model Context Protocol (MCP) de Playwright y úsalo para explorar el filtrado en un navegador.
7. Crea un agente personalizado de control de calidad (QA) y úsalo para revisar los requisitos, la cobertura y las pruebas de verificación.
8. Revisa todo el cambio de filtrado y usa Agent Merge para su PR.
9. Explora comandos slash útiles para gestionar el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube.

Para mantener el taller centrado, crearás dos PR: una para las valoraciones y otra para el filtrado, las actualizaciones de instrucciones y aptitudes, el perfil de QA y las pruebas. El filtrado y el flujo de trabajo de calidad comparten una conversación y una rama para que aproveches el trabajo previo al explorar cada herramienta.

## Lecciones

| Lección | Tema | Descripción |
| ------ | ----------- | ----------- |
| [0. Requisitos previos][ex0] | Configuración | Crea el repositorio y Codespace |
| [1. Instalar Copilot CLI][ex1] | Instalación | Instala y autentica Copilot CLI y familiarízate con él |
| [2. Añadir valoraciones: un cambio rápido][ex2] | Primer cambio | Muestra las valoraciones existentes y el valor alternativo nulo; después, combina tu primer PR |
| [3. Modos de agente: Plan y Autopilot][ex3] | Modos de agente | Planifica la funcionalidad desde su incidencia, créala con Autopilot y revísala en Interactive |
| [4. Guía a Copilot con instrucciones personalizadas][ex4] | Contexto | Explora y actualiza las instrucciones y aplícalas al filtrado |
| [5. Personalizar y usar una aptitud de comprobaciones de calidad][ex5] | Comprobaciones repetibles | Explora la aptitud existente, cambia el formato del informe y ejecútala |
| [6. Validar la funcionalidad con Playwright MCP][ex6] | Observación en el navegador | Configura MCP en la CLI e inspecciona el comportamiento del filtrado |
| [7. Crear y usar un agente de QA][ex7] | Requisitos y cobertura | Crea y selecciona un perfil especialista y reúne las pruebas finales de verificación |
| [8. Crear y combinar el PR de la funcionalidad][ex8] | Revisión y combinación | Revisa el cambio completo, crea el PR y usa Agent Merge |
| [9. Comandos slash de GitHub Copilot CLI][ex9] | Funcionalidades de la CLI | Explora el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube |
| [10. Conclusiones y siguientes pasos][ex10] | Resumen | Repasa el flujo de trabajo, las personalizaciones reutilizables y otros recursos |
| [Opcional: Incorporar Foundry][foundry] | Agentes alojados | Prepara un modelo, despliega un agente basado en un catálogo y conéctalo al sitio web |

## Requisitos previos

Antes de participar en este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Permiso para crear un repositorio y un Codespace
- [ ] Conocimientos básicos sobre el terminal o la línea de comandos

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis a través de [GitHub Education][student-plan]. El plan **Copilot Student** incluye el agente, MCP, la revisión de código y las funciones de Copilot CLI que usa este taller.

> [!NOTE]
> Si usas Copilot Business o Copilot Enterprise, comprueba que el administrador haya habilitado Copilot CLI.

## Empezar

**[Empieza por los requisitos previos →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
