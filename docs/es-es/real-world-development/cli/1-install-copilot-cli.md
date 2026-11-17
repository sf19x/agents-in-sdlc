---
title: "Lección 1 - Instalar GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en tu Codespace, familiarízate con la herramienta y localiza la incidencia de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un asistente de programación con agentes que se ejecuta en el terminal. Permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas desde la línea de comandos. Puedes delegar tareas, solicitar cambios y mantener la concentración. El primer paso es instalar la herramienta; puedes hacerlo con herramientas que ya conoces.

En esta lección:

- instalarás GitHub Copilot CLI con npm.
- iniciarás sesión con tu cuenta de GitHub.
- confiarás en el repositorio del taller y probarás una conversación.
- localizarás la incidencia de filtrado con el servidor GitHub MCP integrado.

## Escenario

Tu equipo está empezando a usar agentes de IA para abordar un backlog cada vez mayor. Copilot CLI incorpora esas capacidades al terminal. En esta lección instalarás la herramienta, te autenticarás y la prepararás para el resto del taller.

## Instalar Copilot CLI

Puedes instalar Copilot CLI con [npm][install-cli], WinGet o Homebrew. GitHub Codespaces ya incluye Node.js, así que utilizarás npm.

1. Vuelve a tu Codespace y abre un terminal.
2. Comprueba que Node.js está instalado y cumple el requisito de versión:

   ```bash
   node --version
   ```

   Deberías ver la versión 24 o posterior.

3. Instala Copilot CLI de forma global:

   ```bash
   npm install -g @github/copilot
   ```

4. Comprueba la instalación:

   ```bash
   copilot --version
   ```

   Deberías ver el número de versión.

## Autenticarse en GitHub

La primera vez que inicies Copilot CLI, te pedirá que te autentiques con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se te solicita, sigue las instrucciones del código de dispositivo para autenticarte y autorizar Copilot CLI.
3. Copilot CLI muestra este prompt:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Confirma que la ruta corresponde al repositorio de Tailspin Toys y responde que sí seleccionando **Yes, and remember this folder for future sessions**.

> [!NOTE]
> En un Codespace, es posible que ya estés autenticado mediante tu sesión de GitHub. Si Copilot CLI se inicia sin pedir autenticación, puedes continuar.

## Familiarizarte con la herramienta

Los comandos que escribes en el prompt habitual del shell se ejecutan directamente en Codespaces. Cuando se inicia Copilot CLI, el lenguaje natural se envía al agente y los comandos slash controlan la conversación.

1. Escribe `/model`, usa las teclas de dirección para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsar <kbd>Enter</kbd> para confirmar.
2. Escribe `/help` para ver los comandos disponibles en tu versión y pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz una pregunta sencilla para comprobar que todo funciona:

   ```plaintext
   What are the key files in this project?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de responder.
5. Escribe `/mcp list` y confirma que está disponible el servidor GitHub Model Context Protocol (MCP) integrado.
6. Pide a Copilot que encuentre la incidencia de filtrado:

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. Abre la URL y lee la incidencia. La utilizarás después de completar un primer cambio rápido.

> [!TIP]
> Una sesión normal de Copilot CLI trabaja en la rama seleccionada en el terminal; no crea automáticamente un worktree. Crearás una rama de funcionalidad antes de cada cambio.

## Usar el atajo del taller

Normalmente Copilot CLI pide permiso antes de usar herramientas fuera de los permisos establecidos. Para centrarte en los ejercicios, volverás a iniciarlo con `--yolo`, un atajo aprobado por el usuario que elimina esos avisos dentro del Codespace.

> [!CAUTION]
> `--yolo` habilita los permisos automáticos completos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Úsalo solo en un entorno aislado, como Codespaces o una máquina virtual, y nunca lo configures como alias predeterminado para el trabajo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning] para obtener más información.

Para este taller, `--enable-all-github-mcp-tools` habilita las herramientas GitHub MCP de lectura y escritura que se utilizarán más adelante para trabajar con incidencias y PR. Codespaces limita el acceso a tu equipo local, pero los recursos de GitHub autenticados siguen siendo reales. Revisa los cambios antes de publicarlos o combinarlos.

1. Sal de Copilot CLI con `/exit`.
2. Reinícialo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta rápida sobre el proyecto para confirmar que la conversación funciona y, después, sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, cuando cambies las instrucciones o añadas un agente, utilizarás `copilot --resume` para volver a la misma conversación y rama de funcionalidad.

## Resumen y siguientes pasos

En esta lección:

- has instalado GitHub Copilot CLI con npm.
- te has autenticado con tu cuenta de GitHub.
- has confiado en el repositorio del taller y probado una conversación.
- has localizado la incidencia de filtrado mediante el servidor GitHub MCP integrado.

A continuación, [realizarás tu primer cambio específico][next-lesson] y utilizarás Copilot CLI para mostrar la valoración por estrellas en las tarjetas de los juegos.

## Recursos

- [Instalar GitHub Copilot CLI][install-cli]
- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
