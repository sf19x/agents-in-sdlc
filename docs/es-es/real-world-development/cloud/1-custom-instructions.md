---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Requisitos previos][previous-lesson] · [Siguiente ejercicio: Agente en la nube de Copilot →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información de fondo, asegúrate de que pueda acceder a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan esa guía para que Copilot entienda no solo *qué* debe hacer, sino también *cómo*.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones específicos de rutas.
- añadirás un estándar general del repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no compararás prompts antes y después. El agente en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que su influencia es más difícil de mostrar en tiempo real. La verás más adelante, al revisar las PR que genere.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices para las prácticas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe usar el modo oscuro y tener un aspecto moderno.
- El código debe incluir comentarios de documentación TypeScript (TSDoc).
- Cada archivo debe empezar con un bloque de comentarios que describa su propósito.

Utilizarás archivos de instrucciones para proporcionar a Copilot la información necesaria para seguir estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que se ajuste mejor a tu estilo de programación y requisitos. Puedes especificar convenciones, bibliotecas y tipos de comentarios. También puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos.

Hay dos tipos de archivos:

- `.github/copilot-instructions.md` se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información pertinente para la mayoría de las solicitudes, como la pila tecnológica, una descripción del proyecto, buenas prácticas y directrices generales.
- `.github/instructions/*.instructions.md` contiene instrucciones para tareas o tipos de archivo concretos, como TypeScript, Astro, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En un entorno de desarrollo integrado (IDE), los archivos de instrucciones se utilizan para generar código en Copilot Chat, no para completar código ni para sugerir ediciones siguientes.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` (con frontmatter `applyTo`) al generar código.
>
> Copilot también [admite otros formatos de archivos de instrucciones][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar las instrucciones

Los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en el proyecto, su estructura y los estándares generales.
- Usa `*.instructions.md` para instrucciones específicas de tipos de archivo (pruebas unitarias, componentes Astro, capa de datos) o tareas.
- Usa lenguaje natural, directrices claras y ejemplos del código que se debe crear o evitar.

No hay una única forma de crear archivos de instrucciones. Descubrirás qué funciona mejor mediante la experimentación.

> [!TIP]
> Cada proyecto que usa GitHub Copilot debería tener una colección sólida de archivos de instrucciones. En este proyecto encontrarás archivos para varias tareas, como [actualizaciones de interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El taller de cada entorno indicará cuándo utilizarla.
>
> Para buscar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/

## Explorar los archivos de instrucciones personalizados del proyecto

Lee los archivos de instrucciones incluidos en el repositorio: hay un `copilot-instructions.md` principal y varios `*.instructions.md` para tareas distintas.

1. Abre `.github/copilot-instructions.md`.
2. Explora la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las instrucciones anidadas sobre **GitHub Actions Workflows**.
3. Abre `.github/instructions` y explora los archivos para Astro, la capa de datos de Drizzle, las pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md` y fíjate en el campo `applyTo`, que define un glob relativo a la raíz del repositorio. En este caso, coincide con archivos de prueba TypeScript como `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa sus enlaces a otros archivos de instrucciones y archivos del proyecto. Así puedes dividir las instrucciones extensas en archivos reutilizables y señalar ejemplos. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás reglas para comentarios TSDoc y encabezados de archivo.

## Añadir un estándar del repositorio

Añade una regla general para que el código incluya comentarios TSDoc y un encabezado con la descripción del archivo. El agente en la nube la utilizará en las incidencias que le asignes más adelante.

1. En el terminal del Codespace, crea una rama nueva:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Vuelve a abrir `.github/copilot-instructions.md`.
3. Busca la sección **Code formatting requirements**, cerca de la línea 27. Todavía no incluye una regla de documentación en el código.
4. Añade estas líneas debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. Guarda `copilot-instructions.md`.

> [!TIP]
> Las instrucciones generales del proyecto pertenecen a `.github/copilot-instructions.md`; las instrucciones específicas de un lenguaje, tipo de archivo o tarea pertenecen a `*.instructions.md`.

## Confirmar, enviar y combinar la actualización

El agente en la nube lee las instrucciones de la rama a la que apunta la incidencia. En el siguiente ejercicio, el agente creará una rama desde `main`; por eso, combina estas instrucciones en `main` para que pueda utilizarlas.

1. Prepara y confirma el cambio:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una PR desde `custom-instructions-cloud` a `main` y combínala.

> [!TIP]
> Si prefieres trabajar directamente en `main`, puedes omitir la rama y confirmar el cambio directamente en `main`. La rama permite practicar un flujo de trabajo habitual.

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones y has añadido un estándar general a `.github/copilot-instructions.md`. Verás su efecto en las PR que genere el agente durante el resto del recorrido.

A continuación, [asignarás tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
