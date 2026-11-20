---
title: "Ejercicio 2 - Añade funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios sencillos pueden requerir modificaciones en varios archivos, pruebas y depuración. El modo de agente de Copilot puede explorar el proyecto, realizar cambios coordinados, ejecutar tareas como las pruebas y corregir errores que detecte. En este ejercicio lo usarás para añadir una funcionalidad.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en tu IDE y permite ciclos de respuesta rápidos. El agente en la nube trabaja de forma asíncrona con incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo Copilot explora el proyecto, identifica archivos relacionados e implementa funcionalidades en la interfaz y la capa de datos; también revisarás los cambios y las pruebas antes de combinarlos.

## Escenario

El catálogo de Tailspin Toys crece y quieres que los usuarios puedan filtrar por categoría y editorial. En el ejercicio anterior añadiste un helper de editoriales; ahora completarás la capa de datos, la interfaz y las pruebas con el modo de agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio para conocer su funcionalidad. Es una plataforma de financiación colectiva de juegos de mesa con una temática para desarrolladores. Se trata de una sola aplicación Astro que genera páginas HTML estáticas en el momento de la compilación. Las páginas consultan una base de datos SQLite local mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz del lado del cliente. Los helpers de acceso a datos se encuentran en `src/lib/` y la interactividad se implementa con scripts Astro acotados que utilizan las API DOM estándar.

### Inicia el sitio

1. Vuelve a tu Codespace; después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, el terminal mostrará una URL similar a esta:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

5. Mantén pulsada <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor.

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir una ventana del navegador para `http://localhost:4321`, ciérralo seleccionando **x**.
>
> En Codespaces, la URL localhost del terminal se redirige a `https://<your-codespace-name>-4321.app.github.dev/`, un túnel privado al servidor del Codespace.

### Explora el sitio

La página principal muestra una lista de juegos con títulos, imágenes y descripciones. Al seleccionar un juego, se abre su página de detalles con información adicional, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> En el Codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. Cada ejercicio indica el modo y el modelo que debes seleccionar.

Al crear el proyecto desde la plantilla se generó automáticamente un backlog de incidencias.

1. En el selector de agentes de Chat, selecciona **Agent**, que puede planificar y aplicar cambios en varios archivos, ejecutar comandos y utilizar herramientas.
2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> La inclusión de este modelo no implica una preferencia de los autores. Se utilizó al crear el taller para que las sugerencias fueran relativamente coherentes. Los modelos de lenguaje son probabilísticos, por lo que las sugerencias pueden variar; es normal.

> [!NOTE]
> Debido a la naturaleza probabilística de los modelos, Copilot podría utilizar un comando MCP diferente y aun así completar la tarea.

3. Pide a Copilot que muestre el backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que muestre las incidencias y revisa la lista priorizada.

## Revisa los archivos de instrucciones

Antes de generar código, revisa el [archivo de instrucciones de interfaz][ui-instructions] en `.github/instructions/ui.instructions.md`. Incluye información sobre la arquitectura y los principios de diseño, pruebas y accesibilidad, además de enlaces a instrucciones para Astro, Tailwind CSS y Drizzle.

## Implementa el filtrado

La funcionalidad necesita cambios en la capa de datos (`src/lib/`), pruebas de filtrado y la página de listado de juegos. Copilot puede realizar estas tareas.

1. Continúa la conversación actual o selecciona **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui**.

   ![Selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que el agente **Agent** sigue seleccionado.

   ![Selector de agente de la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que está seleccionado **Claude Sonnet 4.5**.
5. Envía este prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto, localiza los helpers, la interfaz y las pruebas, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Diálogo de Copilot Chat para confirmar la ejecución de pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que algunas pruebas fallen durante el proceso; Copilot itera entre la generación de código y las pruebas hasta resolver los errores. Mientras trabaja, podrías ver los controles **Keep** y **Undo** en el editor. No hace falta seleccionarlos hasta que Copilot termine.

   ![Sesión completa de Copilot Chat con el modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)
9. Revisa el código generado.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador y explora la nueva funcionalidad.
11. Cuando hayas revisado el código y confirmado que funciona, selecciona **Keep** en Copilot Chat.

## Resumen y próximos pasos

Has usado el modo de agente de GitHub Copilot para añadir funcionalidad al sitio Tailspin Toys. Copilot exploró el proyecto, identificó archivos y coordinó cambios en la interfaz y la capa de datos. Has revisado los cambios y las pruebas antes de combinarlos.

A continuación, [probarás la funcionalidad con Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
