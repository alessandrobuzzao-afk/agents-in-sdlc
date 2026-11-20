---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações não enfrentem problemas com dívida técnica. Isso pode incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos que ficaram paradas no backlog por falta de tempo para implementá-los. O agente de nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina o trabalho, ele cria uma pull request de rascunho para que um desenvolvedor possa revisar. Assim, é possível delegar tarefas tediosas, acelerar o desenvolvimento e liberar os desenvolvedores para se concentrarem em questões mais abrangentes.

Neste exercício, você explorará o agente de nuvem do Copilot para:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- entender a importância de issues com escopo bem definido.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas para resolver. Os prestadores de serviços contratados inicialmente para criar a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, ela está completamente ausente. Como primeiro passo, a equipe gostaria de adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design está pronta para aprimorar a descoberta de jogos. Ela quer que a página de detalhes de cada jogo mostre títulos relacionados — outros jogos da mesma categoria — para que as pessoas continuem navegando. Ainda não é necessário criar uma implementação refinada; basta algo que a equipe possa usar para testes de aceitação da experiência do usuário. Essa tarefa está atualmente bloqueada, mas há outras issues de prioridade maior.

Esses são dois exemplos de tarefas que podem perder prioridade com facilidade e são ótimas candidatas para atribuir ao agente de nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona enquanto o desenvolvedor se dedica a outras tarefas e, depois, retorna para revisar o trabalho e confirmar se está correto.

## Apresentação do agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano de maneira semelhante a um desenvolvedor. E, assim como no trabalho com um desenvolvedor, há várias maneiras de atribuir tarefas, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois de receber a atribuição, o Copilot cria uma pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar na tarefa. Você pode examinar a sessão do Copilot enquanto o trabalho está em andamento ou depois de concluído. Quando a solução proposta estiver pronta para revisão, ele marcará você na pull request!

## A importância de instruções com escopo definido

O GitHub Copilot não faz mágica — você ainda precisa pensar. Até operações aparentemente simples ficam complexas quando analisadas em detalhes. Por isso, [defina com cuidado o escopo das tarefas para o agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a inclusão da IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram instruções personalizadas e como elas orientam o Copilot. Se você já fez um exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; se ainda não fez, este é um bom momento para dar uma olhada rápida. Antes de atribuir trabalho ao agente de nuvem do Copilot, leia os arquivos de instruções já incluídos no repositório sem alterá-los para que você possa observar o efeito deles mais tarde.

Abra os arquivos a seguir na interface Web do GitHub para seu repositório ou em um codespace que já esteja em execução:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, especialmente as expectativas sobre convenções de TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que associa estas instruções aos arquivos `**/*.test.ts`.

Quando atribuir ao agente de nuvem a issue `Code lacks documentation` na próxima seção, observe se a pull request resultante contém comentários TSDoc, segue as convenções de TypeScript e inclui cabeçalhos de comentário. Essas características vêm dos arquivos de instruções. Voltaremos a esse assunto ao revisar as PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do agente de nuvem do Copilot

A criação de código normalmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto — independentemente de quem esteja fazendo o trabalho. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de maneira semelhante a um engenheiro de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente criando um [workflow de configuração especial][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml`, que será executado antes do início do trabalho. Assim, o agente terá acesso às ferramentas de desenvolvimento e dependências necessárias. A configuração já foi preparada para o laboratório para facilitar o fluxo e esta oportunidade de aprendizado. Ela fornece Node.js, dependências do projeto, binários de navegador para testes de ponta a ponta e o banco de dados SQLite migrado e populado para o aplicativo Astro único:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões no nível mínimo necessário para *suas etapas*. O Copilot recebe seu próprio token para as operações dele.
    permissions:
      # Para clonar o repositório como parte da configuração, por exemplo, para instalar dependências, você precisa da permissão `contents: read`. Se não clonar o repositório nas etapas de configuração, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo: Node.js (o aplicativo agora é todo Astro + Drizzle/libSQL)
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

      # Migra e popula o banco de dados SQLite local para que as compilações e os testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo parece qualquer outro workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único trabalho chamado `copilot-setup-steps`. Esse trabalho é executado no GitHub Actions antes de o Copilot começar a trabalhar na pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para confirmar que o workflow funciona, em vez de aguardar o início do trabalho do Copilot.

## Adicione documentação

Todos entendem a importância da documentação, mas muitos projetos têm informações desatualizadas ou não têm documentação. Esse tipo de dívida técnica geralmente fica sem solução, reduzindo a produtividade e dificultando a manutenção da base de código e a integração de novos desenvolvedores. Felizmente, o Copilot é muito eficiente na criação de documentação, então esta é uma issue perfeita para atribuir ao agente de nuvem do Copilot. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o trabalho.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. No campo **Title**, digite `Code lacks documentation`.
6. No campo **Description**, digite:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

   ![Detalhes da atribuição da issue ao Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra a pull request (PR) criada recentemente, que terá um título semelhante a `[WIP]: Code lacks documentation`. Se ela ainda não aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar vários minutos para aparecer na PR. O Copilot está criando o ambiente (executando o workflow mencionado anteriormente), analisando o projeto e determinando a melhor abordagem para resolver o problema.

13. Revise a lista e as tarefas que serão executadas.
14. Role a linha do tempo da pull request para baixo. Você deverá ver uma atualização indicando que o Copilot começou a trabalhar na issue.
15. Selecione **View session**.

   ![Exibição da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão em tempo real e acompanhar como o Copilot resolve o problema. Ele explora o código e entende o estado atual, pausa para pensar e definir o plano adequado e cria código.

Isso provavelmente levará alguns minutos. Um dos principais objetivos do agente de nuvem do Copilot é executar tarefas de forma assíncrona para que você possa se dedicar a outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente de nuvem do Copilot e, em seguida, escreveremos código para adicionar funcionalidades ao aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes do jogo

Como já destacamos, uma das grandes vantagens do agente de nuvem do GitHub Copilot é a possibilidade de dividir o trabalho: você pode se concentrar em um conjunto de tarefas enquanto ele trabalha em outro. Mesmo que adicionar uma seção de jogos relacionados para a equipe de design não demore muito, ainda é um tempo que poderia ser dedicado a outras tarefas. Vamos atribuí-la ao agente de nuvem do Copilot!

1. Retorne ao seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. No campo **Title**, digite `Show related games on the game details page`.
6. No campo **Description**, digite:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você deverá ver o emoji 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela!

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

Agora o Copilot está trabalhando diligentemente na segunda solicitação! O agente de nuvem trabalha de maneira semelhante a um engenheiro de software, então não é necessário acompanhá-lo ativamente. Basta revisar o trabalho quando estiver concluído. Vamos nos concentrar em criar e usar agentes personalizados.

## Resumo e próximos passos

Nesta lição, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para que sejam resolvidas de forma assíncrona. O Copilot pode ajudar a resolver dívidas técnicas, criar novos recursos ou migrar código de uma estrutura para outra.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- entender a importância de issues com escopo bem definido.
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, vamos criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e tem acesso a instruções personalizadas, exploradas em módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
