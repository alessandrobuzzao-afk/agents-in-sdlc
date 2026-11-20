---
title: "Exercício 5: Itere sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Monitoramento e gerenciamento de agentes][previous-lesson] |
|:--|

## Revise o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design explorar e implementasse recursos de acessibilidade, incluindo opções de modo de alto contraste e modo claro. Vamos explorar as alterações e, se necessário, enviar feedback ao Copilot para aprimorar o trabalho.

### Cenário

Como já destacamos várias vezes, os fundamentos de design de software e DevOps não mudam com a inclusão da IA generativa. Sempre precisamos revisar o código gerado e seguir nosso processo normal de DevOps. Com isso em mente, vamos revisar as sugestões do GitHub Copilot para criar a documentação, o recurso de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, foram definidas algumas restrições de segurança para manter tudo protegido. Entre elas:

- O Copilot só tem acesso de leitura ao repositório e acesso de gravação **somente** à ramificação usada para o código.
- O agente de nuvem é executado no GitHub Actions, onde cria um ambiente separado e efêmero para trabalhar.
- Os workflows do GitHub Actions precisam ser aprovados por uma pessoa antes de serem executados.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo servidores MCP.

## Revise a documentação gerada

Vamos começar explorando a primeira pull request (PR) criada pelo agente de nuvem do GitHub Copilot: a que adiciona documentação ao código. Você fará essa tarefa usando a interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao explorar a PR, você talvez veja um aviso de que o firewall bloqueou o GitHub Copilot. Isso é **esperado**, pois o acesso do Copilot a recursos externos é limitado por padrão, incluindo chamadas a servidores MCP externos. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem do Copilot][agent-firewall].

1. Retorne ao seu repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra a pull request intitulada **Add missing documentation** ou outro título semelhante.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, a pull request terá a indicação **[WIP]**. Nesse caso, aguarde até que ele termine. Isso pode levar alguns minutos; aproveite para fazer uma pausa ou refletir sobre o que aprendeu até agora.

4. Quando a pull request estiver pronta, selecione a guia **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui os novos comentários TSDoc e outra documentação. As alterações exatas variam.

   Ao revisar as alterações, procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário. Eles vêm dos arquivos de instruções personalizadas que você revisou no início do Exercício 2.

6. Depois de revisar as atualizações e confirmar que estão corretas, retorne à guia **Conversation** e role para baixo.
7. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão aparecer na seção de verificações da pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.

## Peça alterações ao GitHub Copilot

Trabalhar com o Copilot em uma pull request não é uma interação de mão única. Você pode mencioná-lo em comentários da PR ou em comentários em linha no código, como faria com outros membros da equipe. O Copilot verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados não são determinísticos, não podemos fornecer um texto exato para enviar. Algumas sugestões do que pedir ao Copilot:

- Adicionar cabeçalhos de comentário no início de cada arquivo de código com uma breve descrição do conteúdo.
- Adicionar comentários TSDoc a arquivos TypeScript e Astro.
- Criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**, como faria com qualquer usuário. Use uma das ideias acima ou outra sugestão de documentação que gostaria de ver na base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Você pode selecionar **Back to pull request** para retornar à pull request.

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, deverá aparecer um novo commit na pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Depois, você pode marcar a PR como pronta para revisão e mesclá-la à ramificação principal.

![Marcar a PR como pronta para revisão](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

Vamos retornar à PR criada pelo Copilot para resolver a issue sobre exibir jogos relacionados na página de detalhes de um jogo.

1. Retorne ao seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione a PR com um título semelhante a **Show related games on the game details page**.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações e confirmar que estão corretas, retorne à guia **Conversation** e role para baixo.
6. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows deverão aparecer na seção de verificações da pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** você pode alternar para essa ramificação no Codespace para testar manualmente o recurso de jogos relacionados. Acesse seu Codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da ramificação criada pelo Copilot, por exemplo, **copilot/fix-8**):

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot criou o recurso de jogos relacionados! Como antes, você pode iterar com o agente de nuvem para solicitar atualizações. Por exemplo, peça ao Copilot que ajuste a quantidade de jogos relacionados exibidos ou adicione cabeçalhos de comentário e comentários TSDoc (lembre-se: a tarefa foi atribuída **antes** da atualização das instruções personalizadas!). Como nas outras solicitações, adicione um comentário na guia **Conversation**; o Copilot o verá e iniciará uma nova sessão.

## Revise os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados usando o agente personalizado de acessibilidade. Esta PR deve incluir tanto o modo de alto contraste, atribuído no Exercício 3, quanto o modo claro, solicitado no painel de agentes do Exercício 4.

1. Retorne ao seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione a PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, a pull request terá a indicação **[WIP]**. Nesse caso, aguarde até que ele termine. Isso pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção especial a:
   - componentes da interface para alternar entre os modos.
   - uso do armazenamento local para manter as preferências do usuário.
   - alterações de CSS ou estilo dos modos de alto contraste e claro.
   - atributos de acessibilidade (rótulos ARIA, navegação pelo teclado etc.).
   - código JavaScript/TypeScript que gerencia a alternância entre os modos.

6. Depois de revisar as atualizações e confirmar que estão corretas, retorne à guia **Conversation** e role para baixo.
7. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão aparecer na seção de verificações da pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** alterne para essa ramificação no Codespace para testar manualmente os recursos de acessibilidade. Acesse seu Codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da ramificação criada pelo Copilot):

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Em seguida, inicie o aplicativo e teste os modos de alto contraste e claro no navegador para confirmar se funcionam como esperado e se as preferências permanecem após recarregar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot a implementar esses recursos seguindo as práticas recomendadas de acessibilidade. Se notar algum problema ou oportunidade de melhoria, mencione **@copilot** em um comentário para solicitar atualizações, como fez com as outras PRs.

## Exercício opcional: continue delegando

O agente de nuvem funciona melhor quando você pode delegar itens reais do backlog e se dedicar a outras atividades. Para desenvolver esse hábito, crie mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- Criar um formulário para manifestar interesse em um jogo na página de detalhes.
- Implementar paginação na página de lista de jogos.
- Adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Ao longo destas lições, você:

- **Inspecionou os arquivos de instruções personalizadas incluídos no repositório** para observar mais tarde o efeito deles no trabalho do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** de acessibilidade para adicionar modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como painel de controle** para monitorar e orientar a sessão de acessibilidade em andamento.
- **Revisou e ajustou as pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Para continuar explorando, os outros percursos complementam o que praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, MCP do Playwright, Agent Merge e uma ponte `/delegate` opcional para retornar ao agente de nuvem.

No seu próprio repositório, experimente estas ideias:

- Atribua ao agente de nuvem uma issue de refatoração ou cobertura de testes.
- Crie um agente personalizado para outro domínio.
- Configure `copilot-setup-steps.yml` para outra pilha de tecnologias.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Monitoramento e gerenciamento de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
