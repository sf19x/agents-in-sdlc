---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Muchas organizaciones tienen deuda técnica: problemas de seguridad sin resolver, código heredado que necesita actualizarse o funcionalidades pendientes en el backlog. El agente en la nube de GitHub Copilot puede actualizar código y añadir funciones de forma autónoma. Al terminar, crea una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así puedes delegar tareas tediosas y centrarte en el trabajo de mayor alcance.

En este ejercicio explorarás cómo:

- personalizar el entorno de generación de código.
- asegurar que las operaciones se realicen de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys quiere resolver dos tareas. La primera es añadir comentarios de documentación TypeScript (TSDoc) a todas las funciones exportadas. La segunda es mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego, para que el equipo de diseño pueda probar la experiencia. Son tareas que pueden quedar relegadas y que se pueden asignar al agente para que trabaje de forma asíncrona mientras tú te ocupas de otras actividades.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot][copilot-agents] puede trabajar en segundo plano de forma similar a un desarrollador. Puedes, por ejemplo, [asignarle una incidencia de GitHub][assign-issue]. El agente crea una solicitud de incorporación de cambios (PR) en borrador para registrar el progreso, configura un entorno y empieza a trabajar. Puedes revisar la sesión mientras está activa o cuando haya terminado; cuando la solución esté lista, te mencionará en la PR.

## La importancia de instrucciones bien acotadas

GitHub Copilot no sustituye el análisis y la planificación. Incluso las tareas aparentemente sencillas pueden ser complejas, así que sigue las [recomendaciones para definir el alcance del trabajo del agente en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque.

## Instrucciones personalizadas de este repositorio

Antes de asignar trabajo, inspecciona las instrucciones incluidas en el repositorio para poder reconocer su efecto en las PR.

1. Abre `.github/copilot-instructions.md` en la interfaz web de GitHub o en un Codespace.
2. Revisa la sección **Code standards**, especialmente las convenciones de TypeScript y los comentarios TSDoc.
3. Abre `.github/instructions/unit-tests.instructions.md` y observa que el frontmatter `applyTo` aplica las instrucciones a archivos `**/*.test.ts`.

Cuando asignes la incidencia `Code lacks documentation`, fíjate en los comentarios TSDoc, las convenciones de TypeScript y los encabezados de archivo que aparezcan en la PR. Estas pautas proceden de los archivos de instrucciones que has revisado.

## Configurar el entorno del agente en la nube

El agente utiliza [GitHub Actions][github-actions] para preparar el entorno. Puedes personalizarlo con un flujo de trabajo de configuración en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece. El taller ya incluye uno para instalar Node.js, las dependencias del proyecto, los navegadores de Playwright y la base de datos SQLite migrada y precargada:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Para clonar el repositorio como parte de la configuración, por ejemplo, para instalar dependencias, necesitas el permiso `contents: read`. Si no lo clonas en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (ahora toda la aplicación usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y precarga la base de datos SQLite local para que las compilaciones y pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a otros flujos de GitHub Actions, pero ten en cuenta lo siguiente:

- Contiene un único trabajo llamado `copilot-setup-steps`, que se ejecuta antes de que Copilot empiece a trabajar en la PR.
- El desencadenador `workflow_dispatch` permite ejecutar el flujo manualmente desde la pestaña **Actions** para comprobar que funciona.

## Añadir documentación

Asigna al agente la tarea de añadir documentación al código.

1. Abre el repositorio en github.com en una pestaña nueva.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** y, después, **Blank issue**.
4. Escribe `Code lacks documentation` en **Title**.
5. Escribe esta descripción:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

6. Selecciona **Create** para crear la incidencia.
7. En el lado derecho, selecciona **Assign to Copilot** y, en el cuadro de asignación, selecciona **Assign**.

   ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

   ![Detalles de asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

8. Selecciona la pestaña **Pull Requests** y abre la PR que se acaba de crear, cuyo título será similar a `[WIP]: Code lacks documentation`. Si todavía no aparece, espera un momento y actualiza la página.
9. Cuando Copilot cree una lista de tareas, revísala. Puede tardar varios minutos mientras prepara el entorno, analiza el proyecto y determina un enfoque.
10. Desplázate por la cronología de la PR y confirma que Copilot ha empezado a trabajar en la incidencia.
11. Selecciona **View session** para observar el trabajo del agente. Actualiza la página si no ves el indicador.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

12. Observa cómo Copilot explora el código y determina un plan antes de crear código.

El agente puede tardar varios minutos. Como trabaja de forma asíncrona, puedes ocuparte de otra tarea mientras tanto.

## Añadir una sección de juegos relacionados

Mientras el agente trabaja en la documentación, asígnale otra tarea para que el equipo pueda explorar una mejora de la experiencia.

1. Vuelve al repositorio en github.com y selecciona **Issues**.
2. Selecciona **New issue** y, después, **Blank issue**.
3. Escribe `Show related games on the game details page` en **Title**.
4. Escribe esta descripción:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

5. Selecciona **Create** y, en el lado derecho, selecciona **Assign to Copilot** y, después, **Assign**.

   ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

Poco después, aparecerá 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando en ella.

![Emoji de ojos que indica que Copilot está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

El agente trabaja de forma similar a un desarrollador de software: no hace falta supervisarlo activamente; revisarás el resultado cuando termine. Mientras tanto, explorarás los agentes personalizados.

## Resumen y siguientes pasos

Has explorado el agente en la nube de GitHub Copilot. Puedes asignarle incidencias para que trabaje de forma asíncrona en deuda técnica, funcionalidades o migraciones de código.

En este ejercicio:

- has personalizado el entorno de generación de código.
- has revisado cómo se mantienen seguras las operaciones.
- has visto la importancia de definir el alcance de las incidencias.
- has asignado incidencias a Copilot.

Mientras el agente trabaja, crearás y utilizarás agentes personalizados. El agente en la nube también puede usar [servidores MCP][cloud-agent-mcp] e instrucciones personalizadas.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente ejercicio: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
