---
title: "Lección 6 - Valida la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para explorar el filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Escribir código no es lo único que importa. También trabajamos con datos y servicios externos, y podemos ampliar las automatizaciones disponibles para Copilot. Para ello sirven los servidores MCP: permiten que Copilot utilice herramientas y servicios adicionales a los que ya incluye la CLI.

En esta lección:

- aprenderás qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI;
- añadirás el servidor Playwright MCP si aún no está disponible;
- pedirás al agente que controle un navegador y explore el filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar cambios en la interfaz hace falta interactuar con ella. Quieres que Copilot pueda utilizar el sitio como lo haría una persona y automatizar más comprobaciones.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real para consultar información actualizada y realizar acciones. Estas herramientas y recursos se ofrecen mediante un servidor MCP, que actúa como puente entre el agente y los servicios.

Algunos servidores MCP conocidos son:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: ofrece automatización del navegador mediante Playwright.

GitHub aloja un [registro MCP][mcp-registry] para facilitar el descubrimiento y las contribuciones.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia. Antes de usarlos, revisa su código fuente, verifica quién los publica y considera las implicaciones de seguridad.

## Añade el servidor Playwright MCP

Si Playwright MCP aún no está disponible, añádelo para que Copilot pueda interactuar con el sitio.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve a tu Codespace.
2. En Copilot CLI, abre el cuadro de diálogo para añadir un servidor MCP:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo de servidor **STDIO** con <kbd>Enter</kbd> y pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo.

## Pide a Copilot que explore la funcionalidad con Playwright

Ya has comprobado manualmente que la funcionalidad funciona. Ahora pide a Copilot que haga lo mismo con el servidor Playwright MCP.

1. Usa este prompt para pedirle que valide la funcionalidad:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Normalmente no es necesario indicar que use MCP: Copilot puede averiguarlo por sí mismo. Aun así, especificar la herramienta ayuda a obtener resultados más coherentes y puede ahorrar tokens.

2. Observa los pasos que Copilot realiza en el navegador.
3. Lee el informe y confirma que todo se comporta como esperas.

Copilot iniciará el servidor, interactuará con el sitio mediante Playwright, detendrá el servidor y te entregará un informe.

## Resumen y próximos pasos

Has usado Playwright MCP para explorar la funcionalidad en un navegador real desde Copilot CLI. Aprendiste qué es MCP, añadiste el servidor Playwright si era necesario y pediste al agente que comprobara el filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combinará la habilidad y las herramientas del navegador.

## Recursos

- [Qué es MCP y por qué se habla tanto de ello][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
