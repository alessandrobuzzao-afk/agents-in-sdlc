---
title: "Lição 12 - Implante o site com o GitHub Pages"
description: "Use o aplicativo GitHub Copilot para configurar um fluxo de trabalho do Pages, revisar a pull request e verificar o Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dê ao Space Quiz um endereço público implantando-o com o GitHub Pages. O Copilot pode preparar o fluxo de trabalho e orientar a implantação, mas você continua responsável por revisar as alterações e confirmar que o site está disponível.

Nesta lição, você vai:

- pedir ao Copilot que configure uma implantação do GitHub Pages.
- revisar o fluxo de trabalho e a pull request antes da mesclagem.
- acompanhar a implantação e verificar o site publicado.

## Cenário

O Space Quiz já está em um repositório do GitHub, mas as pessoas ainda precisam de uma maneira simples de experimentá-lo. Publicar o site de um único arquivo com o GitHub Pages o disponibiliza em uma URL pública, sem adicionar um servidor ou uma dependência de runtime.

## Peça ao Copilot que prepare a implantação

> [!WARNING]
> Criar uma pull request pode fazer commit e enviar código. Revise as configurações de aprovação e o repositório de destino antes de enviar este prompt. Se a opção **Approve all** estiver ativada, não espere uma solicitação de confirmação separada.

Na sessão do aplicativo Copilot para `space-quiz`, envie este prompt:

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## Revise o fluxo de trabalho e a pull request

1. Confirme se o README foi adicionado somente se o repositório ainda não tinha um.
2. Revise as alterações da pull request, incluindo as etapas de compilação e implantação do fluxo de trabalho, os gatilhos e as permissões.
3. Confirme se o fluxo de trabalho implanta a partir da ramificação padrão, e não de uma pull request ainda não mesclada.
4. Peça as correções necessárias e revise novamente as diferenças. Aguarde a aprovação das verificações obrigatórias da pull request antes de autorizar a mesclagem.

## Autorize a mesclagem e a implantação

> [!WARNING]
> O prompt a seguir autoriza o agente a mesclar a pull request revisada, alterar as configurações do Pages do repositório e publicar o site. Envie-o somente depois de concluir a revisão e confirmar o repositório e a pull request. As configurações de aprovação podem permitir essas ações sem outra confirmação.

Substitua `<PR_NUMBER>` pelo número da pull request revisada e envie este prompt separado:

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama em três etapas: uma pull request do space-quiz mesclada; uma execução bem-sucedida do GitHub Actions para implantar o site, com as etapas Build website, Upload artifact e Deploy to Pages; e o Space Quiz publicado em yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Verifique a implantação

1. Acompanhe a execução de implantação do GitHub Actions até que ela seja bem-sucedida.
2. Abra a URL informada e confirme se o Space Quiz carrega.

Se o agente não puder habilitar o Pages nem mesclar a pull request, siga as instruções específicas fornecidas por ele. Não considere a tarefa concluída até que a implantação seja bem-sucedida e o site publicado carregue.

## Resumo e próximos passos

Seu Space Quiz agora está disponível como um site publicado, e você verificou a implantação em vez de confiar apenas no relatório do agente. Continue para [Lição 13: Revisão e próximos passos][next-lesson].

[next-lesson]: ../13-review/
