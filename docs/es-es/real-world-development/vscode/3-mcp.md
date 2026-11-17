---
title: "Ejercicio 3 - Probar la funcionalidad con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Modo de agente][previous-lesson] · [Siguiente ejercicio: Agentes personalizados →][next-lesson]

Acabas de crear el filtrado con el modo de agente. Antes de abrir una solicitud de incorporación de cambios (PR), confirma que funciona en el navegador. Añadirás el **servidor Playwright MCP** y permitirás que Copilot controle un navegador real para probar la funcionalidad; después, publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo amplían los servidores MCP las herramientas de Copilot.
- añadirás el servidor Playwright MCP al espacio de trabajo.
- pedirás a Copilot que lo utilice para probar manualmente el filtrado en un navegador.
- publicarás la rama y abrirás una solicitud de incorporación de cambios.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente es más potente cuando puede acceder a recursos externos al editor. Model Context Protocol (MCP) es un estándar que permite al agente comunicarse con herramientas y servicios externos.

![Diagrama del funcionamiento del modo de agente y su interacción con el contexto, el modelo de lenguaje y las herramientas, incluidas las proporcionadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de inteligencia artificial (IA) se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Un servidor MCP actúa como puente entre el agente de IA y las herramientas y servicios externos, como las interfaces de programación de aplicaciones (API) existentes o herramientas locales como los paquetes de npm. Cada servidor ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP populares son:

- **[GitHub MCP Server][github-mcp-server]** proporciona API para gestionar repositorios de GitHub, crear repositorios, actualizar los existentes y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server][playwright-mcp-server]** proporciona automatización del navegador con Playwright para navegar por páginas web, rellenar formularios y seleccionar botones.

GitHub aloja un [registro MCP][mcp-registry] para facilitar la búsqueda y las contribuciones al ecosistema.

> [!CAUTION]
> Por seguridad, trata los servidores MCP como cualquier otra dependencia. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y valora las implicaciones de seguridad. Usa solo servidores de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisar la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu Codespace.
2. Comprueba que ya hay un servidor `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

La plantilla incluye esta entrada `github`, por lo que Copilot pudo leer el backlog en el ejercicio anterior. Utiliza el [servidor remoto GitHub MCP][remote-github-mcp-server]. No tienes que instalarlo localmente: VS Code se conecta mediante el protocolo de transferencia de hipertexto (HTTP) y te autenticas en GitHub la primera vez que Copilot usa una de sus herramientas.

## Añadir el servidor Playwright MCP

Añade un segundo servidor. Playwright MCP proporciona a Copilot un navegador controlable para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Guarda el archivo.

El servidor `playwright` es **local**, a diferencia del servidor remoto `github`: VS Code lo inicia ejecutando `npx @playwright/mcp@latest`. La opción `--headless` ejecuta el navegador sin ventana visible, algo necesario en un Codespace sin escritorio.

> [!NOTE]
> Tailspin Toys ya utiliza Playwright para las pruebas de extremo a extremo, así que el navegador suele estar instalado. Si Copilot informa de que falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Iniciar y confiar en el servidor

VS Code inicia los servidores MCP **cuando se necesitan**. La primera vez que Copilot necesita las herramientas de un servidor, VS Code lo inicia y te pregunta si confías en él.

1. En `.vscode/mcp.json`, selecciona **Start** sobre la entrada `playwright` para iniciar el servidor y comprobar la configuración.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en él.

Iniciarlo manualmente es opcional; si no lo haces, VS Code lo iniciará y solicitará tu confianza la primera vez que Copilot necesite sus herramientas.

## Probar la funcionalidad de filtrado

Playwright MCP proporciona a Copilot un navegador real. El agente puede abrir una página, aplicar filtros, leer los resultados y resumirlos. El servidor utiliza el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de capturas de pantalla. El agente razona sobre elementos estructurados y etiquetados, como botones, enlaces y elementos de lista, igual que hace la tecnología de asistencia; así, una comprobación funcional también sirve como una revisión básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe el filtrado:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador mediante Playwright MCP, seguirá los pasos y resumirá los resultados. Compara el informe con los criterios de aceptación de la incidencia. Si algo no funciona como se espera, haz preguntas de seguimiento o pide que corrija el código antes de abrir una PR.

> [!NOTE]
> La aplicación debe estar en ejecución en `http://localhost:4321`. Si detuviste el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot use Playwright MCP quizá tenga que descargar un navegador; si informa de que falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar la rama y crear una solicitud de incorporación de cambios

Ahora que has confirmado que la funcionalidad funciona, publica la rama `filtering-vscode`.

1. Abre el panel **Source Control** del Codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Panel Source Control con los cambios realizados por Copilot](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crear la solicitud de incorporación de cambios

Copilot puede localizar la incidencia relacionada y crear una PR vinculada a ella.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva.
2. Confirma que **Agent** está seleccionado para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Si es necesario, selecciona **Continue** para permitir que Copilot reúna información y realice las operaciones. La primera vez que utilice una herramienta de GitHub, quizá debas autenticarte; sigue las instrucciones.
5. Observa cómo Copilot busca la incidencia correcta y crea la PR.
6. Selecciona el enlace de Copilot para revisar la PR, pero **no la combines todavía**.

## Resumen y siguientes pasos

En este ejercicio:

- has aprendido qué es MCP y cómo amplían los servidores MCP las herramientas de Copilot.
- has añadido el servidor Playwright MCP al espacio de trabajo.
- lo has utilizado para probar manualmente el filtrado en un navegador.
- has publicado la rama y abierto una PR para el cambio.

Has usado un servidor MCP para probar una funcionalidad, pero MCP permite ampliar Copilot de muchas otras formas. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Servidor GitHub MCP][github-mcp-server]
- [Registro GitHub MCP][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
