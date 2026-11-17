---
title: "Ejercicio 2 - Añadir funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios sencillos de una aplicación suelen implicar editar varios archivos y ejecutar pruebas. El modo de agente de Copilot puede explorar la estructura del proyecto, realizar los cambios necesarios, ejecutar tareas como las pruebas y corregir los errores que detecta. En este ejercicio lo utilizarás para añadir una función al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE y permite ciclos de respuesta rápidos. El agente en la nube trabaja de forma asíncrona en GitHub, como un miembro del equipo, mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo de agente explora el proyecto, identifica archivos y coordina cambios.
- el modo de agente implementa funciones en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados antes de combinar el código.

## Escenario

A medida que crece la lista de juegos, quieres permitir el filtrado por categoría y editor. En el ejercicio anterior añadiste un helper de editores; ahora completarás el resto de la capa de datos, la interfaz y las pruebas con el modo de agente de Copilot.

## Ejecutar el sitio web Tailspin Toys

Antes de cambiar el código, explora el sitio para conocer su funcionalidad actual. Es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. El sitio es una única aplicación Astro que genera lenguaje de marcado de hipertexto (HTML) estático durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle, un mapeador objeto-relacional (ORM); no hay una interfaz de programación de aplicaciones (API) de backend independiente ni un framework de interfaz en el cliente. Los helpers reutilizables de acceso a datos están en `src/lib/`, y la interactividad se implementa con scripts Astro acotados que utilizan las API estándar del modelo de objetos del documento (DOM).

### Iniciar el sitio web

La tarea `npm run dev` inicia el único servidor de desarrollo de Astro.

1. Vuelve al Codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor Astro esté listo, debería mostrar un mensaje similar a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en el navegador, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar el enlace localhost desde el terminal se redirige a `https://<your-codespace-name>-4321.app.github.dev/`, un túnel privado a tu Codespace.

### Explorar el sitio web

Con el sitio abierto, explora las funciones principales:

- **Página de inicio**: muestra juegos de mesa con títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, muestra más información, como el título, la descripción, el editor y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Vuelve al Codespace, abre el panel Copilot Chat y selecciona **New Chat**. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea necesario.

El sitio funciona, pero quieres mejorarlo. La plantilla creó automáticamente un backlog de incidencias. Pide a GitHub Copilot que lo muestre.

1. En el selector de agentes de la vista Chat, selecciona **Agent**, que puede planificar e implementar cambios en varios archivos, ejecutar comandos e invocar herramientas.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos.

> [!CAUTION]
> Los autores no expresan preferencia por un modelo concreto. Se incluye Claude Sonnet 4.5 porque se utilizó al crear el taller, con la intención de mantener sugerencias relativamente coherentes. Los modelos de lenguaje son probabilísticos, por lo que las sugerencias pueden variar; es normal.

> [!NOTE]
> Copilot puede utilizar un comando MCP distinto, pero debería poder completar la tarea.

3. Envía este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que se enumeren las incidencias.
5. Revisa la lista y las prioridades propuestas.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones de interfaz de usuario (UI), que proporciona contexto sobre cómo añadir funciones al sitio.

1. En el Codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa la arquitectura, los principios de diseño de componentes, las pruebas, la accesibilidad y los enlaces a las instrucciones de Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y del proyecto. Las rutas son relativas al archivo de instrucciones, lo que permite reutilizar contenido, dividir instrucciones complejas y proporcionar ejemplos.

## Implementar el filtrado

Para completar el filtrado, la aplicación necesita lógica en la capa de datos (`src/lib/`), pruebas y una interfaz en la página de lista de juegos. Copilot Agent Mode puede trabajar en esas tareas.

1. Continúa en la conversación actual o selecciona **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Selección del archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** está seleccionado.
4. Confirma que el modelo seleccionado es **Claude Sonnet 4.5**.
5. Envía este prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto, localiza los helpers de datos, la interfaz y las pruebas, y después modifica archivos y ejecuta pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot puede tardar varios minutos. Mientras trabaja, quizá aparezcan los controles **Keep** y **Undo** en el editor. No tienes que seleccionarlos mientras el agente sigue trabajando; al terminar, podrás revisar cada cambio.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de Copilot Chat que solicita permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede haber pausas y fallos en algunas pruebas mientras Copilot itera entre la generación de código y las pruebas.
9. Revisa el código generado.

   ![Sesión completa de Copilot Agent Mode](../../../_images/ex3-agent-mode-proposed-changes.png)

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de inteligencia artificial (IA).

10. Vuelve al navegador con el sitio abierto y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y siguientes pasos

Has utilizado el modo de agente de GitHub Copilot para añadir funciones al sitio Tailspin Toys. En este ejercicio has aprendido cómo:

- implementar funciones en la interfaz y la capa de datos.
- explorar el proyecto, identificar archivos y coordinar cambios.
- revisar los cambios y las pruebas antes de combinarlos.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

Cuando crezca la lista de juegos, puede que necesites paginación. Utiliza lo aprendido para pedir a Copilot que la implemente. Decide si el usuario podrá elegir el tamaño de página o si será fijo, proporciona el contexto necesario y, si hace falta, itera con Copilot. Es normal pedir cambios y aportar más contexto.

## Recursos

- [Copilot: modos ask, edit y agent; qué hacen y cuándo se utilizan][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
