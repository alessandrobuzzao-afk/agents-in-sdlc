---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente de Copilot en la nube que realice tres tareas para mejorar la experiencia y añadir funcionalidades. Aunque trabaja de forma asíncrona y autónoma, es importante poder supervisar esas tareas.

GitHub ofrece varias herramientas para gestionar el trabajo asignado al agente, entre ellas la [página de agentes][agents-page]. Desde este centro puedes ver las tareas que tienen solicitudes de incorporación de cambios abiertas, consultar las operaciones realizadas y orientar sesiones en curso.

En esta lección explorarás la página de agentes y guiarás una sesión para pedir una funcionalidad adicional.

## Escenario

Después de pedir al agente el modo de alto contraste, el equipo decide añadir también un modo claro. Como ya se están actualizando los estilos y los controles, pide al agente que incluya esta función en la tarea en curso.

## Revisa las tareas del agente

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa las tareas en los paneles principal e izquierdo. Deberías encontrar tareas para documentar el código, generar API para modificar juegos y añadir el modo de alto contraste.
3. Selecciona una tarea en curso y revisa los pasos que realiza Copilot: obtener el código, crear el entorno, configurar servidores MCP y completar la tarea.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo y el enfoque del agente.

4. Consulta también el panel de solicitudes de incorporación de cambios, a la derecha, para revisar la solicitud y los archivos modificados.

## Guía al agente en la nube

Ahora pedirás a Copilot que añada el modo claro mientras implementa el modo de alto contraste.

1. Selecciona la sesión de alto contraste. El título exacto depende de cómo la haya nombrado Copilot y del estado de la tarea.

   ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Espera a que Copilot termine la configuración y empiece a trabajar.
3. En el diálogo **Steer active session while Copilot is working**, añade este prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Tarea del agente en la nube y diálogo para guiar la sesión activa](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviarlo.
5. Observa cómo Copilot reconoce la petición y la incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot incorporará la nueva petición cuando termine el paso en curso. El proceso puede tardar varios minutos; mientras tanto, puedes repasar lo aprendido.

## Resumen y próximos pasos

Has explorado la página de agentes de Copilot, el centro desde el que se supervisan y guían las tareas del agente en la nube. Puedes revisar las tareas activas y completadas, examinar el trabajo y redirigir una sesión en curso.

A continuación, [iterarás sobre las solicitudes de Copilot][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
