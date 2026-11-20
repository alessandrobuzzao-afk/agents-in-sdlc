---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot son asistentes especializados en tareas o ámbitos concretos del desarrollo. Se definen con archivos Markdown en `.github/agents` del repositorio o globalmente en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con, al menos, los campos `name` y `description`, seguidos de instrucciones que definen el comportamiento, la experiencia y las responsabilidades del agente.

Los equipos pueden codificar sus conocimientos en agentes reutilizables: uno de accesibilidad que aplique [WCAG][wcag], uno de seguridad que siga prácticas seguras o uno de pruebas que mantenga patrones coherentes.

### Agentes personalizados y habilidades de agente

Un **agente personalizado** es el trabajador y las **habilidades** son herramientas. Cada agente tiene su propia ventana de contexto y puede coordinar habilidades e incluso otros agentes. En este taller, el agente de accesibilidad revisará el sitio según las pautas de accesibilidad y podría utilizar habilidades de flujo de PR o de ejecución de pruebas.

> [!NOTE]
> No hay una única manera correcta de crear agentes personalizados. Prueba e itera para encontrar lo adecuado para tu entorno.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas. Algunos usuarios tienen dificultades para leer el tema oscuro por el bajo contraste. El equipo de diseño solicita un modo de alto contraste que se pueda activar y desactivar. Usarás un agente personalizado para implementar la función.

En este ejercicio revisarás un agente de accesibilidad existente y lo usarás desde Copilot Chat para crear el modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente de accesibilidad. Vuelve a tu Codespace, abre un terminal y crea una rama desde `main`:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

3. Revisa las responsabilidades principales, las prácticas recomendadas, los ejemplos HTML, CSS y JavaScript, y los errores habituales.

## Usa el agente personalizado en Copilot Chat

VS Code muestra los agentes definidos en `.github/agents` en el selector situado en la parte inferior de Copilot Chat.

> [!TIP]
> **Abre Copilot Chat**
>
> En el Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. Cada ejercicio indica el modo y el modelo que debes usar.

1. En el selector de agentes, confirma que está seleccionado **Agent**.
2. Abre el selector de agentes de la parte inferior del chat y elige **Accessibility agent**.

   ![Selector de agente de la vista Chat](../../../_images/shared-chat-mode-selector.png)

3. Envía este prompt:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

4. Copilot explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y un diff.
5. Observa cómo se actualiza **Files changed** con los archivos modificados.

> [!NOTE]
> Puede tardar varios minutos. Copilot modificará archivos reales, como componentes Astro, estilos y pruebas relacionadas.

Revisarás y guiarás el trabajo en curso en el próximo ejercicio.

## Resumen y próximos pasos

Has explorado los agentes personalizados de GitHub Copilot y has usado uno en el modo de agente de Copilot Chat. Los agentes permiten convertir la experiencia y los estándares del equipo en asistentes reutilizables.

A continuación, [supervisarás y guiarás el trabajo del agente][next-lesson], revisarás los cambios a medida que se realicen y añadirás un control para el modo claro.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados de awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Prueba tu funcionalidad con Playwright MCP][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
