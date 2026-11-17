---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot son asistentes de inteligencia artificial (IA) especializados para tareas o ámbitos concretos del flujo de desarrollo. Se definen mediante archivos Markdown en `.github/agents` del repositorio y permiten proporcionar a Copilot instrucciones, prácticas recomendadas, patrones de programación y conocimientos específicos. Los equipos pueden convertir su experiencia en agentes reutilizables, como un agente de accesibilidad que aplica las [pautas de accesibilidad para el contenido web (WCAG)][wcag], uno de seguridad o uno de pruebas.

Los agentes personalizados se definen en archivos Markdown dentro de `.github/agents` o, de forma global, en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con, al menos, `name` y `description`, seguido de un prompt Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Agentes personalizados y aptitudes de agente

Los agentes personalizados y las [aptitudes de agente][agent-skills-concept] se parecen en algunos aspectos: ambos se definen principalmente mediante archivos Markdown y explican a una IA cómo realizar tareas. Una forma sencilla de distinguirlos es que el **agente personalizado** es quien realiza el trabajo y las **aptitudes** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y coordinan aptitudes e incluso otros agentes. En este ejercicio, el agente de accesibilidad revisará y actualizará el sitio según las directrices de accesibilidad. Como parte del trabajo, podría utilizar aptitudes para gestionar una solicitud de incorporación de cambios (PR) o ejecutar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como con cualquier herramienta de IA, prueba e itera para encontrar lo que funciona en tu entorno y tus escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

En este ejercicio explorarás:

- cómo se definen los agentes personalizados.
- cómo asignar una tarea a un agente personalizado.

## Escenario

Tailspin Toys se compromete a que todas las personas puedan usar su plataforma de financiación colectiva, independientemente de sus capacidades o preferencias visuales. Los comentarios recientes indican que a algunas personas les cuesta leer el tema oscuro por el bajo contraste entre el texto y el fondo. El equipo de diseño solicita un modo de alto contraste que se pueda activar y desactivar. Utilizarás un agente personalizado para implementar esta función cuanto antes.

## Revisar el agente de accesibilidad

El proyecto ya incluye un agente personalizado de accesibilidad. Revisa su contenido para entender cómo guiará a Copilot.

1. Vuelve a tu Codespace.
2. Abre `.github/agents/accessibility.md`.
3. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

4. Revisa las secciones sobre las responsabilidades principales, las prácticas recomendadas de accesibilidad, ejemplos de lenguaje de marcado de hipertexto (HTML), hojas de estilo en cascada (CSS) y JavaScript, y errores frecuentes.

## Crear y asignar una incidencia

La página de agentes funciona como centro de control: permite asignar tareas al agente en la nube, supervisarlas y proporcionar indicaciones adicionales. Asigna el trabajo para crear el modo de alto contraste.

1. Abre tu repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** y, después, **Blank issue**.
4. Escribe `Add high contrast mode to website` en **Title**.
5. Escribe esta descripción:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

6. Selecciona **Create** para crear la incidencia.
7. En el lado derecho, selecciona **Assign to Copilot**.
8. Selecciona **Accessibility agent** en la lista de agentes personalizados.

   ![Cuadro de asignación del agente en la nube con el agente personalizado Accessibility seleccionado](../../../_images/ex5-select-custom-agent.png)

9. Selecciona **Assign**.

Copilot comenzará a trabajar en segundo plano.

## Resumen y siguientes pasos

Has explorado los [agentes personalizados][custom-agents], asistentes de IA especializados para tareas y ámbitos concretos. Permiten convertir la experiencia y los estándares del equipo en agentes reutilizables.

En este ejercicio:

- has revisado cómo se definen los agentes personalizados.
- has asignado una tarea a un agente personalizado.

Mientras Copilot implementa el modo de alto contraste, aprenderás a [supervisar y dirigir la sesión del agente][next-lesson] desde el centro de control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Prepararse para usar agentes personalizados en una organización][org-custom-agents]
- [Prepararse para usar agentes personalizados en una empresa][enterprise-custom-agents]

---

| [← Ejercicio anterior: Agente en la nube de GitHub Copilot][previous-lesson] | [Siguiente ejercicio: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
