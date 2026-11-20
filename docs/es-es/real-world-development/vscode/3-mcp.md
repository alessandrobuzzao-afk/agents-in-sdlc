---
title: "Ejercicio 3 - Prueba tu funcionalidad con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Has creado la función de filtrado con el modo de agente. Antes de abrir una solicitud de incorporación de cambios, confirma que funciona en el navegador. Conectarás el **servidor Playwright MCP** para que Copilot controle un navegador real y pruebe la funcionalidad; después, publicarás la rama y abrirás la PR.

En este ejercicio aprenderás qué es Model Context Protocol (MCP), añadirás Playwright MCP al espacio de trabajo y pedirás a Copilot que pruebe el filtrado en el navegador.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente es más potente cuando puede acceder a herramientas externas. Model Context Protocol (MCP) es un estándar que permite a Copilot comunicarse con ellas.

![Diagrama del modo de agente y su interacción con el contexto, el modelo de lenguaje y las herramientas, incluidas las proporcionadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real, consulten información actualizada y realicen acciones. Los servidores MCP conectan al agente con recursos como API y herramientas locales.

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)** ofrece API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)** permite automatizar el navegador con Playwright.

GitHub aloja un [registro MCP](https://github.com/mcp) para facilitar el descubrimiento y las contribuciones.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia. Antes de usar uno, revisa su código fuente, verifica quién lo publica y considera las implicaciones de seguridad. Usa solo servidores de confianza y ten cuidado con el acceso a recursos u operaciones sensibles.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP del espacio de trabajo.

1. Abre `.vscode/mcp.json` en tu Codespace. Ya debería estar configurado el servidor `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

La plantilla incluye la entrada `github`, que permitió a Copilot leer el backlog. Utiliza el [servidor MCP remoto de GitHub][remote-github-mcp-server], al que VS Code se conecta por HTTP. La primera vez que Copilot use sus herramientas, tendrás que autenticarte.

## Añade el servidor Playwright MCP

El servidor [Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador controlable para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Guarda el archivo.

`github` es un servidor remoto; `playwright` es local y VS Code lo inicia ejecutando `npx @playwright/mcp@latest`. La opción `--headless` ejecuta el navegador sin ventana visible, necesaria en Codespaces.

> [!NOTE]
> Tailspin Toys ya utiliza Playwright para las pruebas de extremo a extremo, por lo que normalmente el navegador ya está instalado. Si Copilot informa de que falta, pídele que ejecute `npx playwright install chromium`.

## Inicia y autoriza el servidor

VS Code inicia los servidores MCP cuando se necesitan y pide confirmar que confías en ellos.

1. En `.vscode/mcp.json`, selecciona **Start** sobre la entrada `playwright` para iniciar el servidor y comprobar la configuración.
2. Si VS Code solicita confirmación, revisa la configuración y autoriza el servidor.

Este paso es opcional: VS Code iniciará el servidor y pedirá confianza cuando Copilot necesite sus herramientas.

## Prueba la funcionalidad de filtrado

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe la funcionalidad:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

El servidor funciona con el árbol de accesibilidad de la página en lugar de capturas de pantalla. Por eso, esta comprobación funcional también puede servir como una revisión básica de accesibilidad.

> [!NOTE]
> La aplicación debe estar en ejecución en `http://localhost:4321`. Si detuviste el servidor de desarrollo, inícialo de nuevo. La primera vez, Playwright MCP podría necesitar descargar el navegador.

Lee el informe de Copilot y compáralo con los criterios de aceptación de la incidencia. Si algo no funciona, pide aclaraciones o correcciones antes de abrir la PR.

## Publica la rama y crea una solicitud de incorporación de cambios

1. En VS Code, abre **Source Control** y revisa los cambios de Copilot.
2. Prepara los cambios con el icono **+** y genera un mensaje con **Sparkle**.

   ![Panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

3. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la solicitud de incorporación de cambios

1. En Copilot Chat, selecciona **New Chat** y confirma que está seleccionado **Agent**.
2. Pide a Copilot que cree una PR vinculada a la incidencia correcta:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

3. Selecciona **Continue** para permitir las acciones necesarias. La primera vez que se use una herramienta de GitHub, quizá debas autenticarte.
4. Cuando Copilot encuentre la incidencia y cree la PR, selecciona el enlace para revisarla, pero **no la combines todavía**.

## Resumen y próximos pasos

Has aprendido qué es MCP, añadido Playwright MCP al espacio de trabajo, probado el filtrado en el navegador y publicado la rama con una PR. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas concretas como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué se habla tanto de ello][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
