---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan deuda técnica: problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes que llevan tiempo en el backlog. El agente de GitHub Copilot en la nube puede actualizar código y añadir funcionalidades de forma autónoma. Al terminar, crea una solicitud de incorporación de cambios en borrador para que la revise un desarrollador. Así se pueden delegar tareas tediosas y centrarse en otras de mayor alcance.

En este ejercicio explorarás cómo personalizar el entorno, realizar operaciones de forma segura, definir incidencias bien acotadas y asignarlas a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica: el código carece de comentarios TSDoc en funciones exportadas. Además, el equipo de diseño quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles. Ambos trabajos pueden posponerse fácilmente y son buenos candidatos para delegar al agente en la nube; tú podrás revisar el resultado cuando termine.

## Introducción al agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) trabaja en segundo plano de forma parecida a un desarrollador. Por ejemplo, puedes [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Copilot crea una solicitud de incorporación de cambios en borrador, prepara un entorno y empieza la tarea. Puedes consultar la sesión durante el trabajo o al terminar; cuando la solución esté lista para revisión, te mencionará en la solicitud.

## Importancia de acotar bien las instrucciones

GitHub Copilot no sustituye el razonamiento ni la planificación. Incluso las tareas aparentemente sencillas pueden tener complejidad; consulta las [prácticas recomendadas para acotar tareas del agente en la nube][cloud-agent-best-practices]. Trátalo como un asistente de programación: trabaja por etapas, aprende y adapta el enfoque.

## Instrucciones personalizadas de este repositorio

Antes de asignar trabajo, revisa los archivos de instrucciones que ya existen:

- `.github/copilot-instructions.md`: consulta **Code standards**, especialmente las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que aplica estas instrucciones a archivos `**/*.test.ts`.

Cuando asignes la incidencia `Code lacks documentation`, fíjate en los comentarios TSDoc, las convenciones de TypeScript y los encabezados de archivo de la PR resultante.

## Configura el entorno de desarrollo del agente

El agente usa [GitHub Actions][github-actions] para preparar el entorno. Puedes personalizarlo con un [flujo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes del trabajo y puede instalar herramientas y dependencias. El repositorio incluye un flujo preparado que instala Node.js y dependencias, instala navegadores de Playwright y prepara la base de datos SQLite de la aplicación Astro.

Este flujo es un flujo normal de GitHub Actions y puede ejecutarse manualmente desde la pestaña **Actions** mediante el disparador `workflow_dispatch`, lo que permite probar su configuración sin esperar a una sesión del agente.

El archivo de configuración del proyecto tiene este aspecto:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña «Actions» del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Limita los permisos a los mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de la configuración, por ejemplo para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de frontend y aplicación - Node.js (toda la aplicación usa Astro y Drizzle/libSQL)
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

      # Migra y carga datos iniciales en SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El flujo contiene un único trabajo llamado `copilot-setup-steps`, que se ejecuta antes de que Copilot empiece la solicitud. El disparador `workflow_dispatch` permite probarlo manualmente desde **Actions**.

## Asigna una tarea de documentación

1. En una nueva pestaña del navegador, abre el repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el diálogo de creación.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en **Title**.
6. Escribe lo siguiente en **Description**:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot**.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. En el diálogo, selecciona **Assign**.

   ![Detalles de asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva solicitud de incorporación de cambios (PR), cuyo título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer mientras Copilot prepara el entorno (ejecuta el flujo de trabajo descrito antes), analiza el proyecto y determina cómo abordar la tarea.

13. Revisa la lista y las tareas que Copilot va a realizar.
14. Desplázate por la cronología de la PR; deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

   ![Vista de una sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que debas actualizar la ventana para ver el indicador más reciente.

16. Observa la sesión en directo para ver cómo Copilot explora el código, analiza el estado del proyecto, decide el plan adecuado y crea código.

El proceso puede tardar varios minutos. Uno de los objetivos del agente en la nube es trabajar de forma asíncrona para que puedas ocuparte de otras tareas. Aprovecha esta función asignándole otra tarea y, mientras tanto, trabaja en una funcionalidad.

## Añade una sección de juegos relacionados a la página de detalles

El agente puede trabajar en una tarea mientras tú te ocupas de otra. Aunque la sección de juegos relacionados quizá no lleve mucho tiempo, puedes aprovechar ese tiempo para otras tareas. Asígnasela al agente en la nube:

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot**.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. En el diálogo, selecciona **Assign**.

Poco después, aparecerá un conjunto de emojis 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando en ella.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot trabajará en la segunda solicitud. Como lo haría un ingeniero de software, no hace falta supervisarlo activamente; revisarás su resultado cuando termine.

## Resumen y próximos pasos

Has explorado el agente de GitHub Copilot en la nube y le has asignado incidencias para abordar deuda técnica y crear una funcionalidad. También has revisado el entorno de configuración, la importancia de las instrucciones y el alcance de las tareas. El agente puede ayudar a resolver deuda técnica, crear funcionalidades y migrar código de un framework a otro.

Ahora [crearás y utilizarás agentes personalizados][next-lesson]. El agente en la nube también puede usar [servidores MCP][cloud-agent-mcp] e instrucciones personalizadas.

## Recursos

- [Acerca del agente de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
