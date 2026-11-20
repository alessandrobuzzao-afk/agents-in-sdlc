---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante el taller has pedido a GitHub Copilot que documente el código, cree una función de juegos relacionados y añada funcionalidades de accesibilidad: modos de alto contraste y claro. Ahora revisarás los cambios y, si hace falta, darás indicaciones para mejorarlos.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian con la IA generativa. Hay que revisar el código generado y seguir el proceso habitual. Revisarás las propuestas de documentación, juegos relacionados y accesibilidad antes de activar la revisión para el resto del equipo.

## Seguridad y agente de Copilot en la nube

Como el agente trabaja de forma asíncrona y sin supervisión, se aplican medidas de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para el código.
- El agente se ejecuta en GitHub Actions, dentro de un entorno efímero independiente.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de ejecutarlos.
- De forma predeterminada, el acceso a recursos externos está limitado, incluidos los servidores MCP.

## Revisa la documentación generada

1. Abre el repositorio en github.com y selecciona **Pull Requests**.
2. Abre la solicitud de documentación, cuyo título será similar a **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la solicitud tendrá la marca **[WIP]**. Espera a que termine; puede tardar varios minutos.

3. Selecciona **Files changed** y revisa el código, los comentarios TSDoc, las convenciones de TypeScript y los encabezados de archivo. Estos proceden de las instrucciones personalizadas revisadas en el Ejercicio 2.
4. Vuelve a **Conversation**. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Botón Approve and run workflows](../../../_images/shared-approve-workflows.png)

5. Comprueba en la sección de verificaciones que se ejecutan los flujos de trabajo del proyecto Astro y que se superan correctamente.

> [!NOTE]
> Puede aparecer un aviso de que el cortafuegos bloqueó a GitHub Copilot. Es normal: el acceso a recursos externos, incluidas las llamadas a servidores MCP externos, está limitado de forma predeterminada. Puedes [configurar o desactivar el cortafuegos][agent-firewall].

## Pide cambios a GitHub Copilot

Puedes mencionar a Copilot en comentarios de la solicitud o en comentarios en línea del código. Copilot puede iniciar otra sesión para resolverlos. Por ejemplo, pídele que añada encabezados de archivo, comentarios TSDoc en archivos TypeScript y Astro o un README que explique la estructura de la aplicación.

1. Añade un comentario y menciona **@copilot**.
2. Selecciona **View Session** para observar la nueva sesión.
3. Usa **Back to pull request** para volver a la solicitud.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, revisa el nuevo commit en **Files changed**.

Puedes iterar hasta quedar conforme. Después, marca la PR como lista para revisión y combínala.

![Convertir una PR en lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

1. En **Pull Requests**, abre la solicitud **Show related games on the game details page**.
2. Selecciona **Files changed** y revisa el código.
3. Vuelve a **Conversation** y aprueba los flujos de trabajo pendientes con **Approve and run workflows**.

   ![Botón Approve and run workflows](../../../_images/shared-approve-workflows.png)

4. En la sección de verificaciones, confirma que los flujos del proyecto se completan correctamente.
5. **Opcional:** cambia a la rama de Copilot en tu Codespace para probar la función. Sustituye `<branch-name>` por la rama creada por Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Puedes pedir a Copilot cambios adicionales mediante comentarios en la pestaña **Conversation**, como ajustar el número de juegos relacionados o añadir comentarios TSDoc.

## Revisa las funciones de accesibilidad

La PR del agente personalizado de accesibilidad debe incluir el modo de alto contraste del Ejercicio 3 y el modo claro solicitado en el Ejercicio 4.

1. Abre la PR titulada **Add high contrast mode to website**.
2. Si aparece **[WIP]**, espera a que Copilot termine.
3. Selecciona **Files changed** y revisa la implementación: controles de modo, almacenamiento local, estilos y atributos de accesibilidad, navegación por teclado y código que gestiona los modos.
4. En **Conversation**, aprueba los flujos de trabajo pendientes y confirma que se superan las comprobaciones.

   ![Botón Approve and run workflows](../../../_images/shared-approve-workflows.png)

5. **Opcional:** cambia a la rama en tu Codespace y prueba que los controles de alto contraste y modo claro funcionan y se mantienen al recargar la página.

Si detectas problemas de accesibilidad, menciona a **@copilot** en un comentario y solicita los cambios.

## Ejercicio opcional: sigue delegando

Presenta otras incidencias en el repositorio y asígnalas a Copilot. Algunas ideas:

- Crear un formulario para expresar interés en un juego.
- Implementar paginación en la página de juegos.
- Añadir validación de datos de entrada y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube: inspeccionaste las instrucciones personalizadas, asignaste incidencias al agente y observaste cómo preparaba el entorno y trabajaba en segundo plano; creaste y usaste un agente de accesibilidad; supervisaste y guiaste su sesión desde la página de agentes; y revisaste e iteraste sobre sus solicitudes, aprobando los flujos de trabajo.

## Repaso y próximos pasos

Los demás recorridos complementan lo que has practicado:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración con MCP en el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con Plan, Autopilot, habilidades, agentes de QA, Playwright MCP, Agent Merge y delegación opcional a la nube.

En tu propio repositorio, puedes asignar incidencias de refactorización o cobertura de pruebas, crear agentes personalizados para otros ámbitos o configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente][agent-firewall]

---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
