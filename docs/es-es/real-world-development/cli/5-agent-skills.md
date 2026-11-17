---
title: "Lección 5 - Personalizar y usar una aptitud de comprobaciones de calidad"
description: "Explora la aptitud quality-checks existente, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escribir código implica más que programarlo. Ya has comprobado manualmente que funciona y has utilizado archivos de instrucciones para que siga los estándares del equipo. Ahora explorarás las pruebas, el lint y otras partes de la integración continua (CI). Las **aptitudes de agente** ayudan a Copilot a realizar tareas repetibles como estas.

En esta lección:

- explorarás la aptitud `quality-checks` existente.
- personalizarás el formato de los resultados.
- volverás a cargar y ejecutar la aptitud.

## Escenario

Tailspin Toys utiliza la aptitud `quality-checks` para las pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las aptitudes de agente reúnen instrucciones reutilizables para tareas, scripts ejecutables y recursos auxiliares que el agente carga cuando los necesita. Una aptitud se organiza en una carpeta con un archivo Markdown `SKILL.md`. El archivo incluye frontmatter con el nombre y la descripción, una explicación de la tarea y pautas sobre cuándo utilizarla. La carpeta también puede incluir scripts y otros recursos.

> [!NOTE]
> No es obligatorio incluir carpetas y archivos adicionales. La aptitud `quality-checks` de Tailspin Toys solo contiene `SKILL.md`, porque utiliza los comandos existentes del proyecto.

Las aptitudes pueden estar en `.github/skills` para compartirlas con el equipo o en la carpeta de aptitudes del usuario `~/.copilot/skills`.

## Explorar la aptitud

Explora la aptitud `quality-checks` que creó el equipo de Tailspin Toys.

1. Vuelve a tu Codespace y abre `.github/skills/quality-checks/SKILL.md` en el editor.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a decidir cuándo llamar a la aptitud.
3. Lee las instrucciones y observa cómo guían a Copilot durante las pruebas y el lint.
4. Comprueba que aún no contiene una sección **Results output formatting**.

## Ejecutar la aptitud antes de cambiarla

Pide a Copilot que ejecute las tres comprobaciones de la aptitud.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Anota el informe final.

## Personalizar el informe

Añade instrucciones para que el informe indique qué se ejecutó, si se completó correctamente y qué resultados comunicaron las herramientas.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade al final del archivo esta sección:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecutar la aptitud actualizada

Copilot CLI puede volver a cargar las aptitudes editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía el mismo prompt de antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Anota el informe y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y utilizado una aptitud de agente existente. En esta lección:

- has explorado la aptitud `quality-checks` para las pruebas unitarias, lint y comprobaciones de tipos.
- has personalizado el formato de sus resultados.
- has vuelto a cargar y ejecutar la aptitud.

El cambio acompañará al filtrado en la PR de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio mediante el [servidor Playwright MCP][next-lesson].

## Más ejemplos de aptitudes

Estos ejemplos comunitarios son referencias, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación: `drawio`][drawio-example]
- [Pruebas del navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
