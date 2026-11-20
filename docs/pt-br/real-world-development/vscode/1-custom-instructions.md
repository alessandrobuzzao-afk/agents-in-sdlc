---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser realizada de uma maneira específica — ou se há informações de contexto que o Copilot deve conhecer —, é importante garantir que ele possa acessá-las. Há várias maneiras de compartilhar um contexto específico com o Copilot. Uma das principais são os [arquivos de instruções][instruction-files], que fornecem orientações sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instruções associados a caminhos.
- gerar a primeira fatia de dados para a filtragem (um auxiliar de editoras) usando as instruções atuais.
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`.
- executar novamente o mesmo prompt e observar o código gerado adotar o novo padrão.
- fazer commit das instruções atualizadas e da fatia de filtragem em `main` para que o Copilot use as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir alguns dos padrões definidos. O Copilot é não determinístico. O objetivo deste exercício é observar a *tendência* de mudança no comportamento após atualizar as instruções, não obter resultados idênticos caractere por caractere.

## Arquivos de instruções

### Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Eles incluem:

- A camada de dados sempre precisa de testes de unidade.
- A interface do usuário deve usar o tema escuro e ter uma aparência moderna.
- O código deve incluir documentação na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever a finalidade dele.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para realizar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas permitem fornecer contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Esse recurso ajuda a orientar o Copilot para que ele sugira código mais relevante. Você pode especificar convenções de programação, bibliotecas e até os tipos de comentários que prefere. É possível criar instruções para todo o repositório ou para tipos específicos de arquivos, a fim de fornecer contexto no nível da tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em **todas** as solicitações feitas no repositório. Ele deve conter informações sobre o projeto — contexto relevante para a maioria das solicitações feitas no chat ou na CLI. Isso pode incluir a pilha de tecnologias, uma visão geral do que está sendo desenvolvido, práticas recomendadas e orientações gerais.
- `.github/instructions/*.instructions.md`: arquivos que podem ser criados para tarefas ou tipos de arquivo específicos. Você pode usá-los para definir orientações para linguagens específicas (como TypeScript ou Astro) ou para tarefas, como criar um componente de interface ou um novo conjunto de testes de unidade.

> [!NOTE]
> Ao trabalhar no IDE, os arquivos de instruções só são usados para gerar código no Copilot Chat — não para conclusões de código nem sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam tanto as instruções no nível do repositório quanto os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [aceita arquivos de instruções em outros padrões][custom-instructions-support], incluindo arquivos AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre a criação de arquivos de instruções está fora do escopo deste workshop. No entanto, os exemplos do projeto mostram uma abordagem representativa. Em linhas gerais:

- Mantenha o arquivo `copilot-instructions.md` focado nas orientações do projeto, como uma descrição do que está sendo desenvolvido, a estrutura do projeto e os padrões gerais de programação.
- Use arquivos `*.instructions.md` para fornecer instruções específicas para tipos de arquivo (testes de unidade, componentes Astro e camada de dados, por exemplo) ou tarefas específicas.
- Use linguagem natural e mantenha as orientações claras. Dê exemplos de como o código deve (e não deve) ser escrito.

Não há uma única maneira específica de criar arquivos de instruções, assim como não há uma única maneira de usar IA. Experimente para descobrir o que funciona no seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Ao explorar os arquivos deste projeto, você verá instruções para vários tipos de tarefas, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa funcionalidade de uma maneira diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI); a lição da interface utilizada indicará quando ela for relevante.
>
> Quer encontrar modelos ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório repleto de arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções personalizadas deste projeto

Reserve um momento para ler os arquivos de instruções incluídos neste repositório: há um arquivo principal `copilot-instructions.md` e uma coleção de arquivos `*.instructions.md` para várias tarefas. Abra esses arquivos no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Na seção **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Essas informações se aplicam a todas as interações com o Copilot.
3. Abra a pasta `.github/instructions` e navegue pelos arquivos. Observe as instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define um padrão glob (relativo à raiz do repositório) que determina a quais arquivos as instruções se aplicam. Neste exemplo, ele corresponde a qualquer arquivo de teste TypeScript, como os que seguem o padrão `**/*.test.ts`.
5. Observe as instruções específicas para criar testes de unidade neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Assim, você pode dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e apontar o Copilot para exemplos. (Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.)

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de programação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário nos arquivos.

## Crie uma ramificação para as alterações

Vamos seguir algumas práticas recomendadas e criar uma ramificação para as alterações.

1. Retorne ao codespace do exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma ramificação e alterne para ela:

   ```bash
   git checkout -b custom-instructions
   ```

## Use o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, retorne ao codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção do modelo variam por exercício — cada etapa indica o que importa.

Para observar o impacto das instruções personalizadas, comece enviando um prompt com o arquivo de instruções atual. Depois, atualize-o e envie o mesmo prompt novamente para comparar os resultados.

1. Feche as guias do editor abertas nos exercícios anteriores para que o Copilot use somente o contexto desejado.
2. Abra `src/lib/publishers.ts` para mostrar ao Copilot onde o auxiliar deve ficar.
3. Selecione **Agent** na lista de agentes da exibição Chat para que o Copilot possa aplicar alterações aos arquivos.

   ![Captura de tela mostrando o seletor de agentes na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie o prompt a seguir:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explora o projeto e aplica atualizações ao código, normalmente abrangendo o arquivo do auxiliar e os testes.
6. Observe que o auxiliar proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras — essas convenções vêm de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observe que o código proposto **não tem** comentários TSDoc nem cabeçalho de comentário no nível do arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele adicione comentários de documentação mesmo sem essa orientação. Tudo bem — o objetivo é observar a melhoria de *consistência* depois da atualização das instruções.

## Adicione um novo padrão ao repositório

Como destacado anteriormente, `.github/copilot-instructions.md` fornece informações do projeto ao Copilot. Vamos documentar os padrões de programação do repositório para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar próxima da linha 27. Observe como ela documenta os padrões de programação do projeto — mas ainda não há uma regra para documentação no código, motivo pelo qual o auxiliar gerado não tinha comentários de documentação.
3. Adicione as linhas Markdown a seguir logo abaixo dos padrões existentes para orientar o Copilot a adicionar cabeçalhos de comentário nos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados no nível do repositório (`.github/copilot-instructions.md`) para definir orientações gerais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo no nível do repositório é o local adequado para padrões do projeto, como a regra de comentários de documentação que você acabou de adicionar.

## Execute novamente o prompt e observe a alteração

1. Retorne ao Copilot Chat e selecione **New Chat** para limpar o histórico.
2. Selecione `src/lib/publishers.ts` para direcionar o Copilot ao arquivo correto.
3. Envie o **mesmo prompt** de antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a:

   ```typescript
   /**
    * Auxiliares de acesso a dados de editoras da plataforma de financiamento coletivo Tailspin Toys.
    * Fornece funções para recuperar informações de editoras do banco de dados.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a:

   ```typescript
   /**
    * Retorna uma lista de todas as editoras com seus IDs e nomes.
    *
    * @param db - Cliente de banco de dados do Drizzle.
    * @returns Uma promessa que é resolvida com uma matriz de objetos de editora.
    */
   ```

Você acabou de orientar o Copilot a seguir um novo padrão e aplicá-lo ao código real que servirá de base para o próximo exercício.

## Faça commit das instruções e envie a ramificação

Os arquivos de instruções são recursos do repositório, assim como qualquer outro arquivo, e são gerenciados pelo mesmo processo de controle do código-fonte. Vamos fazer commit e enviar a ramificação.

1. Abra uma nova janela do terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. No terminal, confirme se as instruções atualizadas e as alterações do auxiliar estão presentes executando:

   ```bash
   git status
   ```

3. No terminal, prepare e faça commit das instruções e de todos os arquivos alterados pelo Copilot para criar a base do auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras atualizações relacionadas (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. No terminal, envie a ramificação ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Crie e mescle uma pull request

Com a ramificação enviada, vamos criar uma pull request e associá-la à issue sobre o padrão de documentação no backlog. Poderíamos fazer isso manualmente, mas o Copilot pode cuidar dessa tarefa com as ferramentas do GitHub já conectadas ao projeto. Vamos pedir que ele localize a issue, crie uma PR que a encerre e, depois, faça a mesclagem.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executa ações no GitHub por você. O modelo do projeto já conecta as ferramentas do GitHub ao espaço de trabalho, então não é preciso configurar nada. Na primeira vez que o Copilot usar uma delas, o VS Code talvez peça que você entre no GitHub. Siga as instruções para permitir o acesso. Você aprenderá como essa conexão funciona (Model Context Protocol) em um exercício posterior.

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a exibição Copilot Chat e confirme se **Agent** está selecionado na lista de agentes.
3. Peça ao Copilot que localize a issue relacionada à atualização dos arquivos de instruções e crie uma PR a partir da ramificação atual, descrevendo tanto as atualizações das instruções quanto a nova base do auxiliar de editoras:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. O Copilot começará a localizar a issue e criar a PR.
5. Quando solicitado a permitir que o Copilot execute ações no GitHub em seu nome, revise o comando e selecione **Allow** se for apropriado.
6. Depois que a PR for criada, peça ao Copilot que a mescle e retorne sua ramificação local para `main` usando o prompt a seguir:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Quando solicitado a permitir que o Copilot execute ações no GitHub e comandos do shell em seu nome, revise o comando e selecione **Allow** se for apropriado.

Você acaba de criar e mesclar uma pull request com a ajuda do GitHub Copilot!

## Resumo e próximos passos

Você explorou como o Copilot usa o contexto dos arquivos de instruções deste projeto e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções *existentes*.
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`.
- executar novamente o mesmo prompt e observar o código proposto adotar o novo padrão.
- fazer commit das instruções e da base do auxiliar em `main` para que o próximo exercício possa continuar o trabalho.

Em seguida, você colocará essas instruções em prática no [modo de agente][next-lesson], enquanto o Copilot adiciona um novo recurso à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
