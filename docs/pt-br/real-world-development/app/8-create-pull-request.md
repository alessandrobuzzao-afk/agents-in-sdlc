---
title: "Lição 8 - Crie e mescle a PR do recurso"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie uma PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

A implementação da filtragem, as atualizações das instruções e da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. É hora de revisar tudo em conjunto e abrir um pull request. Você fez o merge do pull request (PR) de avaliações por estrelas por conta própria; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações das instruções e da skill e o agente de QA em algumas PRs separadas. Para simplificar o workshop, você manteve todo o fluxo de filtragem e qualidade em uma sessão e branch, e incluirá todo esse trabalho nesta PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionar a PR completa do recurso e as evidências de verificação.
- autorizar o Agent Merge somente depois da revisão e confirmar se a PR foi mesclada.

## Cenário

Ao longo do fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora a Tailspin Toys quer automatizar o trabalho restante da PR, mantendo a autorização para fazer merge sob o controle da pessoa desenvolvedora.

## Apresentação do Agent Merge

O **Agent Merge** automatiza o trabalho restante para concluir uma pull request no aplicativo GitHub Copilot. Quando você o habilita, a sessão do aplicativo lê a PR e resolve o que estiver bloqueando o merge — corrigindo verificações de integração contínua (CI) com falha, respondendo a comentários da revisão e fazendo rebase quando necessário — e faz o merge assim que o GitHub permite. Ele trabalha em segundo plano, continua funcionando após reinicializações do aplicativo e é desativado automaticamente quando a pull request é mesclada.

Até agora, você selecionou **Merge pull request** por conta própria. O Agent Merge pode assumir essa responsabilidade, mas ainda precisa de sua autorização explícita para editar código e fazer merge. Revise as ações permitidas e o trabalho antes de conceder permissão para fazer merge.

## Use o Agent Merge para gerenciar a PR

Com todo o código criado e revisado, vamos permitir que o Agent Merge gerencie o processo da PR.

1. Use a lista de agentes para selecionar **Default agent**.
2. Selecione o menu suspenso ao lado de **Create PR**.
3. Selecione **Agent Merge**. O botão mudará para **Agent Merge**.
4. Selecione o botão **Agent Merge** para iniciar o processo. Selecionar somente a opção no menu suspenso não o inicia.

O processo do Agent Merge começa. Ele vai:

- Criar a pull request com título e descrição.
- Se você iniciou a sessão a partir de uma issue, incluir uma referência à issue relacionada no corpo da descrição.
- Fazer rebase ou resolver possíveis conflitos de merge com a branch de destino.
- Acompanhar o processo de CI para garantir que todas as verificações sejam aprovadas.
- Acompanhar a PR em busca de feedback de outras pessoas desenvolvedoras ou da revisão de código do Copilot e atualizar o código para responder a esses comentários.
- Opcionalmente, fazer merge automático da PR quando tudo estiver concluído.

Vamos permitir que o Agent Merge também faça o merge da PR quando tudo estiver aprovado!

5. Revise o diff da PR e as evidências de verificação antes de conceder permissão para fazer merge.
6. Selecione o menu suspenso ao lado de **Agent Merge**.
7. Confirme se há uma marca ao lado de **Merge pull request**.
8. Peça explicitamente ao agente que faça o merge:

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. Acompanhe o progresso e resolva os bloqueios informados. Confirme se o status da PR é **Merged** antes de continuar; habilitar a permissão de merge ou ter um agente em execução não confirma que a mesclagem foi concluída.

> [!IMPORTANT]
> O Agent Merge não ignora as proteções do repositório nem as permissões ausentes. Resolva esses bloqueios antes de continuar.

## Resumo e próximos passos

Você automatizou várias etapas do processo de desenvolvimento, incluindo a geração, os testes e a validação do código e, agora, o processo da pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionou a PR completa do recurso e as evidências de verificação.
- autorizou o Agent Merge somente depois da revisão e confirmou se a PR foi mesclada.

Em seguida, você [usará um canvas existente e criará um canvas de triagem][next-lesson] para explorar uma forma mais completa de examinar, planejar e visualizar o trabalho com o agente.

## Recursos

- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
