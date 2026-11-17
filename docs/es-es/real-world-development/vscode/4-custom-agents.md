---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot son asistentes de inteligencia artificial (IA) especializados para tareas o ámbitos concretos del flujo de desarrollo. Se definen mediante archivos Markdown en `.github/agents` y proporcionan a Copilot instrucciones, prácticas recomendadas, patrones de programación y conocimientos específicos. Los equipos pueden convertir su experiencia en agentes reutilizables, como uno de accesibilidad que aplique las [pautas de accesibilidad para el contenido web (WCAG)][wcag], uno de seguridad o uno de pruebas.

Los agentes personalizados se definen en archivos Markdown en `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con, al menos, `name` y `description`, seguido de un prompt que define el comportamiento, la experiencia y las instrucciones.

### Agentes personalizados y aptitudes de agente

Los agentes personalizados y las [aptitudes de agente][agent-skills-concept] se parecen en algunos aspectos: ambos se definen principalmente mediante archivos Markdown y explican a una IA cómo realizar tareas. Una forma sencilla de distinguirlos es que el **agente personalizado** es quien realiza el trabajo y las **aptitudes** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y coordinan aptitudes e incluso otros agentes. En este ejercicio, el agente de accesibilidad revisará y actualizará el sitio según las directrices de accesibilidad. Puede utilizar aptitudes para gestionar una solicitud de incorporación de cambios (PR) o ejecutar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Prueba e itera para encontrar lo que funciona en tu entorno y tus escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades o preferencias visuales. Algunos usuarios tienen dificultades para leer el tema oscuro porque el contraste entre el texto y el fondo es insuficiente. El equipo de diseño solicita un modo de alto contraste que se pueda activar y desactivar.

En este ejercicio aprenderás cómo:

- revisar un agente personalizado de accesibilidad existente.
- utilizarlo en Copilot Chat para implementar un modo de alto contraste.

## Revisar el agente de accesibilidad

El proyecto ya incluye un agente personalizado de accesibilidad. Revisa su contenido para entender cómo guiará a Copilot.

1. Vuelve a tu Codespace y abre un terminal.
2. Crea una rama nueva desde `main` para el trabajo de accesibilidad, separada de la PR de filtrado del ejercicio 3:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

3. Abre `.github/agents/accessibility.md`.
4. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

5. Revisa las responsabilidades principales, las prácticas de accesibilidad, los ejemplos de lenguaje de marcado de hipertexto (HTML), hojas de estilo en cascada (CSS) y JavaScript, y los errores frecuentes.

## Usar el agente personalizado en Copilot Chat

VS Code muestra en el selector de agentes de Copilot Chat todos los agentes personalizados definidos en `.github/agents`. Puedes seleccionar uno para aplicar sus instrucciones y herramientas a la sesión.

> [!TIP]
> **Abrir Copilot Chat**
>
> Vuelve al Codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; las instrucciones de cada uno lo indicarán cuando sea necesario.

1. En el selector de agentes de la vista Chat, selecciona **Agent** si todavía no está seleccionado.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Abre el selector de agentes de la parte inferior de la vista Chat, que muestra el agente activo (por defecto, **default**).
3. Selecciona **Accessibility agent**.
4. Envía al agente este prompt:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y un diff que puedes revisar.
6. Observa cómo se actualiza **Files changed** con los archivos modificados por el agente.

> [!NOTE]
> Este proceso puede tardar varios minutos. Copilot modificará archivos reales del repositorio, como componentes Astro, CSS y pruebas relacionadas.

Revisarás y dirigirás este trabajo activo en el siguiente ejercicio.

## Resumen y siguientes pasos

Has explorado los [agentes personalizados][custom-agents], asistentes de IA especializados para tareas y ámbitos concretos. Permiten convertir la experiencia y los estándares del equipo en agentes reutilizables.

En este ejercicio:

- has revisado cómo se definen los agentes personalizados.
- has utilizado un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y dirigirás el trabajo del agente][next-lesson], revisarás los cambios mientras se realizan y añadirás un control para el modo claro en la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Ejercicio anterior: Probar la funcionalidad con el servidor Playwright MCP][previous-lesson] | [Siguiente ejercicio: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
