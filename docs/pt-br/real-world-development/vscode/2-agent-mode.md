---
title: "Exercício 2 - Adicione funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até as atualizações mais simples de um aplicativo normalmente exigem alterações em vários arquivos e operações como executar testes. Em geral, o fluxo de trabalho de um desenvolvedor envolve localizar todos os arquivos necessários, fazer as alterações, executar os testes, depurar, descobrir qual arquivo ficou de fora, fazer outra alteração... E a lista continua.

É aí que entra o modo de agente do Copilot.

O modo de agente do Copilot foi criado para trabalhar de forma mais autônoma no IDE. Ele se comporta de maneira semelhante a um desenvolvedor: começa explorando a estrutura existente do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como você pode usar o modo de agente para adicionar novas funcionalidades ao site.

> [!NOTE]
> Apesar dos nomes parecidos, o modo de agente e o agente de nuvem foram criados para experiências diferentes. O modo de agente executa as tarefas no IDE, permitindo ciclos rápidos de feedback e interação. O agente de nuvem trabalha de forma assíncrona como um colega da equipe e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- o modo de agente do GitHub Copilot pode implementar novos recursos na interface e na camada de dados.
- revisar alterações e testes gerados pelo modo de agente do Copilot antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que os usuários filtrem por categoria e editora. Você já adicionou um auxiliar de editoras no exercício anterior; agora, concluirá o restante do trabalho na camada de dados, na interface e nos testes com o modo de agente do Copilot.

## Execute o site Tailspin Toys

Antes de fazer alterações, vamos explorar o site Tailspin Toys para entender a funcionalidade atual.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de programação. Ele permite listar jogos e exibir detalhes sobre eles. O site é um único aplicativo Astro que renderiza páginas HTML estáticas durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, por meio do Drizzle ORM — não há uma API de back-end separada nem uma estrutura de interface no lado do cliente. Os auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é adicionada com um pequeno `<script>` do Astro, de escopo limitado, que usa APIs DOM padrão.

### Inicie o site

Para facilitar a execução do site, foi fornecido o script `npm run dev`, que inicia o único servidor de desenvolvimento do Astro. Você pode executá-lo no GitHub Codespace seguindo estas etapas:

1. Retorne ao seu codespace. Você deverá estar em `main` depois do exercício anterior.
2. Abra uma nova janela do terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie e alterne para uma nova ramificação para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o seguinte comando para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, você verá um banner com a URL, semelhante ao exemplo:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo for aberta perguntando se você quer abrir uma janela do navegador para `http://localhost:4321`, feche-a selecionando o **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, selecionar o link da URL localhost no terminal do Codespace redireciona você automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para seu codespace, que agora está hospedando o servidor web!

### Explore o site

Com o site em execução, você pode explorar as funcionalidades. Os principais recursos incluem:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá uma página com mais informações, incluindo título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, retorne ao codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção do modelo variam por exercício — cada etapa indica o que importa.

A implementação inicial do site funciona, mas queremos aprimorá-la com novos recursos. Vamos começar revisando o backlog. Ao configurar o projeto a partir do modelo, um backlog de issues foi criado automaticamente — peça ao GitHub Copilot que mostre esses itens.

1. Selecione **Agent** na lista de agentes da exibição Chat. O agente **Agent** planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos no terminal e invoca ferramentas.

   ![Captura de tela mostrando o seletor de agentes na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão indicando preferência por um modelo específico. Ao criar o workshop, usamos Claude Sonnet 4.5 e o incluímos nas instruções para que as sugestões de código sejam relativamente consistentes e a experiência seja boa. No entanto, como os LLMs são probabilísticos, as sugestões podem ser diferentes das indicadas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente, mas ainda deverá conseguir concluir a tarefa.

3. Peça ao Copilot que mostre o backlog de issues enviando o prompt a seguir:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista de issues gerada.

Observe como o Copilot também priorizou os itens de acordo com o que considera mais útil para o usuário.

## Revise os arquivos de instruções

Antes de pedir ao agente que gere o código, revise o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo [user interface (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para arquivos de instruções específicos de vários tipos de arquivo, incluindo:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao local do arquivo de instruções. Assim, é possível reutilizar orientações, dividir conjuntos complexos em partes menores e usar exemplos e modelos.

## Implemente a funcionalidade de filtragem

Para concluir a filtragem, serão necessárias pelo menos três alterações separadas no aplicativo:

- Adicionar ou aprimorar a lógica de filtragem restante na camada de dados (`src/lib/`).
- Adicionar ou atualizar testes do comportamento de filtragem.
- Atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes de mesclar tudo à base de código. O modo de agente do Copilot pode realizar essas tarefas por você. Vamos adicionar a funcionalidade.

1. Continue na conversa atual com o Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Captura de tela mostrando a seleção do arquivo de instruções de interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme se **Agent** continua selecionado na lista de agentes da exibição Chat.

   ![Captura de tela mostrando o seletor de agentes na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme se **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade com base na issue relacionada do backlog usando o prompt a seguir:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot enquanto ele explora o projeto e localiza os arquivos relacionados à funcionalidade, incluindo os auxiliares da camada de dados, a interface e os testes. Em seguida, ele modifica os arquivos e executa os testes.

   ![Captura de tela mostrando o Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, os botões **Keep** e **Undo** podem aparecer no editor. Quando o Copilot terminar, você verá **Keep** ou **Undo** para todas as alterações; não é necessário selecioná-los enquanto o trabalho estiver em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela de uma caixa de diálogo no painel Copilot Chat pedindo confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e até testes com falha durante o processo. Tudo bem! O Copilot alterna entre a geração de código e os testes até concluir a tarefa e não detectar erros.

   ![Captura de tela de uma sessão completa do Copilot Chat com o modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado e procure possíveis problemas.

> [!CAUTION]
> Lembre-se: é importante revisar o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Retorne ao navegador com o site em execução e explore a nova funcionalidade!
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, exploramos como usar o modo de agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Aprendemos como:

- o modo de agente do GitHub Copilot pode implementar novos recursos na interface e na camada de dados.
- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- revisar alterações e testes gerados pelo modo de agente do Copilot antes de mesclá-los à base de código.

Agora vamos [testar seu recurso com o servidor MCP do Playwright][next-lesson] e abrir uma pull request.

### Exercício de exploração extra: implemente paginação

À medida que a lista de jogos cresce, será necessário habilitar a paginação. Usando o que você aprendeu neste exercício, peça ao Copilot que implemente a paginação no site. Considere:

- seguir as práticas recomendadas, incluindo o uso dos arquivos de instruções existentes.
- decidir como implementar a paginação: permitir que o usuário escolha o tamanho da página ou definir um valor fixo.
- fornecer ao Copilot as orientações necessárias no prompt para que ele crie a implementação desejada.
- iterar com o GitHub Copilot, pedindo alterações e fornecendo contexto. Esse é o fluxo normal ao trabalhar com o Copilot!

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
