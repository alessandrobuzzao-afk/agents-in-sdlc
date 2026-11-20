---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot son asistentes de IA especializados en tareas o ámbitos concretos del flujo de desarrollo. Se definen con archivos Markdown en `.github/agents` o globalmente en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con al menos `name` y `description`, seguido de instrucciones que describen el comportamiento, la experiencia y las responsabilidades del agente.

Los equipos pueden codificar sus conocimientos en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las pautas [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

### Agentes personalizados y habilidades de agente

Un **agente personalizado** es el trabajador y las **habilidades** son herramientas. Cada agente tiene su propia ventana de contexto y puede coordinar habilidades e incluso otros agentes. En este taller, el agente de accesibilidad revisará el sitio según las pautas de accesibilidad y podría utilizar habilidades de flujo de PR o de ejecución de pruebas.

> [!NOTE]
> No hay una única manera correcta de crear un agente personalizado. Prueba e itera para encontrar lo adecuado para tu entorno.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

En este ejercicio aprenderás cómo se definen agentes personalizados y cómo asignarles tareas.

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas. Los comentarios de usuarios indican que algunas personas tienen dificultades para leer el tema oscuro por el bajo contraste. El equipo de diseño solicita un modo de alto contraste que se pueda activar y desactivar. Usarás un agente personalizado para implementar esta función con rapidez.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente de accesibilidad. Vuelve a tu Codespace y revisa `.github/agents/accessibility.md`:

1. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

2. Revisa las responsabilidades principales, las prácticas recomendadas, los ejemplos HTML, CSS y JavaScript, y los errores habituales.

## Crea y asigna una incidencia

Mission control es el centro desde el que se asignan tareas al agente en la nube, se supervisan sesiones y se proporciona orientación adicional.

1. Abre tu repositorio.
2. Selecciona la pestaña **Issues** y crea una incidencia en blanco.
3. Define el título como `Add high contrast mode to website`.
4. Usa esta descripción:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

5. Selecciona **Create** y, en el panel derecho, **Assign to Copilot**.
6. Selecciona **Accessibility agent** y, después, **Assign**.

   ![Diálogo de asignación de Copilot con el agente de accesibilidad seleccionado](../../../_images/ex5-select-custom-agent.png)

Copilot comenzará a trabajar en segundo plano.

## Resumen y próximos pasos

Has explorado los agentes personalizados de GitHub Copilot, asistentes especializados en tareas y ámbitos concretos. Permiten convertir la experiencia y los estándares del equipo en agentes reutilizables. Has revisado el agente de accesibilidad y le has asignado la creación del modo de alto contraste.

A continuación, [supervisarás y guiarás la sesión del agente][next-lesson] desde mission control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparar una organización para usar agentes personalizados][org-custom-agents]
- [Preparar una empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lección anterior: Agente de GitHub Copilot en la nube][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
