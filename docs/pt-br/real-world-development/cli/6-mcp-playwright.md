---
title: "Lição 6 - Valide a funcionalidade com o MCP do Playwright"
description: "Inspecione ou configure o MCP do Playwright no Copilot CLI e use-o para explorar o recurso de filtragem em um navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já destacamos, escrever código envolve mais do que apenas escrever código. Precisamos trabalhar com dados e serviços externos e até disponibilizar automações adicionais para o Copilot. É aí que entram os servidores MCP. Eles permitem que o Copilot vá além dos recursos integrados à CLI, oferecendo ainda mais ferramentas e serviços.

Nesta lição, você vai:

- entender o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza.
- adicionar o servidor MCP do Playwright, caso ele ainda não esteja disponível.
- pedir ao agente que controle um navegador e explore seu recurso de filtragem.

## Cenário

Os testes de unidade e de ponta a ponta são importantes, mas validar alterações na interface do usuário exige interagir com ela. Você quer permitir que o Copilot use o site em que está trabalhando como um usuário faria, para automatizar ainda mais o processo de alteração e aumentar a confiança de que as atualizações funcionam como esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] oferece aos agentes de IA uma maneira de se comunicar com ferramentas e serviços externos. Com o MCP, os agentes de IA podem se comunicar com ferramentas e serviços externos em tempo real. Assim, eles podem acessar informações atualizadas e executar ações em seu nome.

Essas ferramentas e esses recursos são acessados por meio de um servidor MCP, que funciona como uma ponte entre o agente de IA e as ferramentas e os serviços externos. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Alguns servidores MCP conhecidos são:

- **[Servidor MCP do GitHub][github-mcp]**: fornece acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Servidor MCP do Playwright][playwright-mcp-server]**: oferece recursos de automação de navegadores usando o Playwright.

Há muitos outros servidores MCP disponíveis. O GitHub mantém um [registro de MCP][mcp-registry] para facilitar a descoberta de servidores e as contribuições ao ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do seu projeto. Antes de usar um servidor, revise o código-fonte, verifique o publicador e considere as implicações de segurança.

## Adicione o servidor MCP do Playwright

Se o servidor MCP do Playwright ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como um usuário faria.

> [!NOTE]
> Se o MCP do Playwright já estiver configurado no Copilot CLI, ignore estas etapas e continue na próxima seção.

1. Retorne ao seu Codespace.
2. Abra a caixa de diálogo para adicionar um servidor MCP digitando o seguinte comando no Copilot CLI:

   ```plaintext
   /mcp add
   ```

3. No campo de nome, digite `playwright` e pressione <kbd>Tab</kbd>.
4. Pressione <kbd>Enter</kbd> para confirmar o tipo de servidor **STDIO** e, em seguida, pressione <kbd>Tab</kbd>.
5. Cole o seguinte na caixa de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o novo servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo do MCP.

## Peça ao Copilot que explore o recurso com o Playwright

Antes, você confirmou manualmente que a funcionalidade se comportava como esperado. Agora vamos pedir ao Copilot que faça o mesmo usando o servidor MCP do Playwright!

1. Use o prompt a seguir para pedir ao Copilot que valide a funcionalidade com o servidor MCP do Playwright:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Não é necessário dizer ao Copilot que use o servidor MCP, pois ele geralmente o identificará por conta própria. Mas, como você sabe qual ferramenta ele deve usar, é sempre bom indicar! Isso ajuda a obter resultados mais consistentes e economiza alguns tokens.

2. Observe o Copilot enquanto ele lista as etapas que está executando no navegador para confirmar se a funcionalidade está funcionando.
3. Leia o relatório e confirme se tudo se comporta como esperado.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, parará o servidor e apresentará um relatório.

## Resumo e próximos passos

Parabéns, você usou o servidor MCP do Playwright para explorar seu recurso em um navegador real a partir do Copilot CLI! Para recapitular, você:

- aprendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza.
- adicionou o servidor MCP do Playwright, caso ele ainda não estivesse disponível.
- pediu ao agente que controlasse um navegador e explorasse seu recurso de filtragem.

Em seguida, você [criará um agente personalizado de QA][next-lesson] que combinará a skill e as ferramentas do navegador em uma função especializada.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
