---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Requisitos previos][previous-lesson] · [Siguiente ejercicio: Modo de agente →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información de fondo, asegúrate de que pueda acceder a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan directrices para la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos específicos de rutas.
- generarás la primera parte de la capa de datos para el filtrado (un helper de editores) con las instrucciones actuales.
- añadirás un estándar general a `.github/copilot-instructions.md`.
- repetirás el prompt y observarás cómo el código generado sigue el nuevo estándar.
- confirmarás los cambios en `main` para que Copilot use las instrucciones actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares que establezcas. Copilot no es determinista. El objetivo es observar la tendencia del cambio después de actualizar las instrucciones, no obtener una salida idéntica carácter por carácter.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices para sus prácticas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe usar el modo oscuro y tener un aspecto moderno.
- El código debe incluir comentarios de documentación TypeScript (TSDoc).
- Cada archivo debe empezar con un bloque de comentarios que describa su propósito.

Utilizarás archivos de instrucciones para proporcionar a Copilot el contexto necesario para aplicar estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas permiten proporcionar a Copilot contexto y preferencias para que se ajuste mejor a tu estilo y requisitos. Puedes especificar convenciones, bibliotecas y tipos de comentarios, y crear instrucciones para todo el repositorio o para tipos concretos de archivo.

Hay dos tipos de archivos:

- `.github/copilot-instructions.md` se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información pertinente para la mayoría de las solicitudes, como la pila tecnológica, una descripción del proyecto, buenas prácticas y directrices generales.
- `.github/instructions/*.instructions.md` contiene directrices para tareas o tipos de archivo concretos, como TypeScript, Astro, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En un IDE, los archivos de instrucciones se utilizan para generar código en Copilot Chat, no para completar código ni para sugerir ediciones siguientes.
>
> Copilot Chat, Copilot CLI y el agente en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` (con frontmatter `applyTo`) al generar código.
>
> Copilot también [admite formatos de instrucciones de otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar las instrucciones

Los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en el proyecto, su estructura y los estándares generales.
- Usa `*.instructions.md` para instrucciones específicas de tipos de archivo (pruebas unitarias, componentes Astro, capa de datos) o tareas.
- Usa lenguaje natural, directrices claras y ejemplos del código que se debe crear o evitar.

No hay una única forma de crear archivos de instrucciones. Descubrirás qué funciona mejor mediante la experimentación.

> [!TIP]
> Cada proyecto que usa GitHub Copilot debería tener una colección sólida de archivos de instrucciones. En este proyecto encontrarás archivos para varias tareas, como las [actualizaciones de la interfaz de usuario (UI)][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI.
>
> Para buscar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/

## Explorar los archivos de instrucciones del proyecto

Lee los archivos incluidos en el repositorio: un `copilot-instructions.md` principal y varios `*.instructions.md` para tareas distintas.

1. Abre `.github/copilot-instructions.md`.
2. Explora la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las instrucciones anidadas para **GitHub Actions Workflows**.
3. Abre `.github/instructions` y explora los archivos para Astro, la capa de datos de Drizzle, las pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md` y fíjate en el campo `applyTo`, que define un glob relativo a la raíz del repositorio. En este caso, coincide con archivos de prueba TypeScript como `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa sus enlaces a otros archivos de instrucciones y archivos existentes del proyecto. Así puedes dividir las instrucciones extensas en archivos reutilizables y señalar ejemplos. Las rutas son relativas al archivo de instrucciones.

> [!NOTE]
> La sección **Code formatting requirements** describe los estándares del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás reglas para comentarios TSDoc y encabezados de archivo.

## Crear una rama para los cambios

Crea una rama para trabajar en la funcionalidad.

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat antes de actualizar las instrucciones

Para observar el efecto de las instrucciones, envía primero un prompt con las instrucciones actuales. Más adelante actualizarás el archivo y repetirás el prompt.

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar, vuelve al Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; las instrucciones de cada uno lo indicarán.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot reciba solo el contexto necesario.
2. Abre `src/lib/publishers.ts` para indicar dónde debe estar el helper.
3. En la vista Chat, selecciona **Agent** en el selector de agentes para que Copilot pueda cambiar archivos.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explora el proyecto y aplica cambios, normalmente al helper y sus pruebas.
6. Observa que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editores. Esto se debe a las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto le faltan comentarios TSDoc y un encabezado a nivel de archivo.

> [!CAUTION]
> Copilot podría añadir comentarios de documentación incluso sin instrucciones. No pasa nada; el objetivo es observar la mejora de coherencia después de actualizar las instrucciones.

## Añadir un estándar del repositorio

Añade reglas de documentación en el código para mejorar las sugerencias de Copilot.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, cerca de la línea 27.
3. Añade estas líneas debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Las instrucciones generales del proyecto pertenecen a `.github/copilot-instructions.md`; las específicas de lenguajes, tipos de archivo o tareas pertenecen a `*.instructions.md`.

## Repetir el prompt y observar el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat**.
2. Selecciona de nuevo `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía el mismo prompt de antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque similar a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta incluye un comentario TSDoc similar al siguiente:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has guiado a Copilot para que aplique un estándar del proyecto a código que se utilizará en el siguiente ejercicio.

## Confirmar las instrucciones y enviar la rama

Los archivos de instrucciones se gestionan como los demás recursos del repositorio. Confirma los cambios y envía la rama.

1. Abre un terminal nuevo con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba los cambios:

   ```bash
   git status
   ```

3. Prepara y confirma las instrucciones y todos los archivos modificados por Copilot para el helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados, como pruebas, añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y combinar una solicitud de incorporación de cambios

Vincula la rama con la incidencia sobre los estándares de documentación y pide a Copilot que cree y combine la PR.

> [!NOTE]
> Esta es la primera vez que Copilot actúa en GitHub. La plantilla ya conecta las herramientas de GitHub con el espacio de trabajo; VS Code puede pedirte que inicies sesión cuando Copilot las use por primera vez. Más adelante aprenderás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat en el Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado.
3. Pide a Copilot que busque la incidencia relacionada y cree una PR desde la rama actual:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Cuando Copilot te pida permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** según corresponda.
5. Cuando se cree la PR, pide a Copilot que la combine y vuelva a `main`:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Cuando Copilot te pida permiso para realizar acciones de GitHub o ejecutar comandos de shell, revisa el comando y selecciona **Allow** según corresponda.

Has creado y combinado una PR con la ayuda de GitHub Copilot.

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y, con Copilot Chat en VS Code:

- has enviado un prompt de generación de código y observado el resultado con las instrucciones existentes.
- has añadido un estándar general a `.github/copilot-instructions.md`.
- has repetido el prompt y observado cómo el código propuesto sigue el nuevo estándar.
- has confirmado las instrucciones y el helper en `main` para que el siguiente ejercicio pueda utilizarlos.

A continuación, aplicarás esas instrucciones en el [modo de agente][next-lesson] mientras Copilot incorpora una nueva funcionalidad en el código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas de usuario para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
