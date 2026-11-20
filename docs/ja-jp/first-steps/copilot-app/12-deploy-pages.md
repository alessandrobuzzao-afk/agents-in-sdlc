---
title: "演習 12 - GitHub Pages を使ってサイトをデプロイする"
description: "GitHub Copilot app を使って Pages ワークフローを設定し、pull request を確認して公開済みの Space Quiz を検証します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages を使って、Space Quiz を公開サイトとして公開します。Copilot はワークフローの準備とデプロイ手順の案内を行えますが、変更の確認とサイトの公開を確認する責任は開発者が負います。

この演習では、次のことを行います。

- Copilot に GitHub Pages のデプロイ設定を依頼する。
- workflow と pull request をマージ前に確認する。
- デプロイを追跡し、公開されたサイトを検証する。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、利用者が簡単に試せる方法が必要です。単一ファイルで構成されたサイトを GitHub Pages に公開すると、サーバーやランタイムの依存関係を追加せずに、公開 URL から利用できるようになります。

## Copilot にデプロイ準備を依頼する

> [!WARNING]
> pull request を作成すると、コードのコミットと push が行われる場合があります。このプロンプトを送る前に、承認設定とデプロイ先リポジトリを確認してください。**Approve all** を選択した場合は、個別の確認プロンプトが表示されないことがあります。

`space-quiz` の Copilot app セッションで、次のプロンプトを送ります。

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## workflow と pull request を確認する

1. リポジトリに README がない場合だけ README が追加されていることを確認します。
2. pull request の変更内容を確認します。workflow のビルド手順、デプロイ手順、トリガー、権限を含めます。
3. workflow がマージ前の pull requestではなく、デフォルト ブランチからデプロイすることを確認します。
4. 修正が必要な場合は依頼し、更新された差分を確認します。マージを承認する前に、必要な pull request チェックがすべて成功するまで待ちます。

## マージとデプロイを承認する

> [!WARNING]
> 次のプロンプトは、エージェントにマージ、リポジトリの Pages 設定変更、サイト公開を承認するものです。確認を完了し、リポジトリと pull request を確認した後だけ送ってください。承認設定によっては、追加の確認プロンプトが表示されずにこれらの操作が実行される場合があります。

`<PR_NUMBER>` を確認済みの pull request の番号に置き換えてから、次のプロンプトを別途送ります。

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![3段階の図: マージされた space-quiz pull request、Build website、Upload artifact、Deploy to Pages のステップを含む成功した GitHub Actions Deploy website 実行、ライブの Space Quiz（yourname.github.io/space-quiz/）を示しています。](../../../_images/first-steps-app-pages-deploy.svg)

## デプロイを検証する

1. GitHub Actions のデプロイ実行を追跡し、成功するまで待ちます。
2. 報告された URL を開き、Space Quiz が読み込まれることを確認します。

エージェントが Pages を有効化できない、または pull request をマージできない場合は、エージェントが示す具体的な手順に従ってください。デプロイが成功してライブサイトが読み込まれるまで、タスクは完了としないでください。

## まとめと次のステップ

Space Quiz をライブサイトとして公開し、エージェントの報告だけに頼らずデプロイを検証できました。次は [Lesson 13: Review and next steps][next-lesson] に進んでください。

[next-lesson]: ../13-review/
