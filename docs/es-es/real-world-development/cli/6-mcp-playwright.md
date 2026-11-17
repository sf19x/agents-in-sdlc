---
title: "Lección 6 - Validar la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para explorar el filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Además de escribir código, hay que trabajar con datos y servicios externos y proporcionar a Copilot herramientas adicionales. Los servidores Model Context Protocol (MCP) amplían las capacidades integradas en la CLI con herramientas y servicios.

En esta lección:

- aprenderás qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI.
- añadirás el servidor Playwright MCP si todavía no está disponible.
- pedirás al agente que controle un navegador y explore la funcionalidad de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar cambios en la interfaz hace falta interactuar con ella. Quieres que Copilot use el sitio web como lo haría una persona y compruebe el comportamiento.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real, accedan a información actualizada y realicen acciones en tu nombre.

Los agentes acceden a estas herramientas y recursos mediante un servidor MCP, que actúa como puente. Cada servidor ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP populares son:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a las API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: proporciona funciones de automatización del navegador con Playwright.

GitHub aloja un [registro MCP][mcp-registry] para facilitar la búsqueda y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usarlos, revisa su código fuente, comprueba quién los publica y valora las implicaciones de seguridad.

## Añadir el servidor Playwright MCP

Si el servidor Playwright MCP aún no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría una persona.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve a tu Codespace.
2. En Copilot CLI, abre el cuadro de diálogo para añadir un servidor MCP:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo de servidor **STDIO** con <kbd>Enter</kbd> y pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo.

## Pedir a Copilot que explore la funcionalidad con Playwright

Ya has comprobado manualmente que la funcionalidad funciona. Ahora pide a Copilot que haga lo mismo con el servidor Playwright MCP.

1. Envía este prompt:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Normalmente Copilot detectará por sí mismo que debe usar MCP. Mencionarlo explícitamente ayuda a orientar la solicitud y puede hacer que los resultados sean más coherentes.

2. Observa los pasos que Copilot realiza en el navegador para comprobar la funcionalidad.
3. Lee el informe y confirma que todo se comporta como se espera.

Copilot iniciará el servidor, utilizará Playwright para interactuar con el sitio, detendrá el servidor y presentará un informe.

## Resumen y siguientes pasos

Has utilizado Playwright MCP para explorar la funcionalidad en un navegador desde Copilot CLI. En esta lección:

- has aprendido qué es MCP y cómo lo utiliza Copilot CLI.
- has añadido el servidor Playwright MCP si no estaba disponible.
- has pedido al agente que controle un navegador y explore el filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combinará la aptitud y las herramientas del navegador.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
