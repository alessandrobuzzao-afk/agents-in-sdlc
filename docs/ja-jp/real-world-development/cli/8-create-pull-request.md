---
title: "演習 8 - feature PR を作成してマージする"
description: "フィルタリング、instructions、skill の更新、QA プロファイル、テストをまとめて確認し、PR を作成して Agent Merge を使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、instruction の更新、skill の更新、quality assurance (QA) プロファイル、テストが1つのブランチに保存されています。次は、変更をまとめて確認し、pull request を作成します。star-rating の pull request (PR) は開発者自身がマージしましたが、今回は **Agent Merge** にプロセスを管理させます。

> [!NOTE]
> 通常は、機能、instruction の更新、skill の更新、QA agent を別々の PR に分けます。ワークショップを効率化するため、フィルタリングと品質ワークフロー全体を1つの conversation とブランチにまとめ、この PR に含めています。

この演習では、次のことを行います。

- Agent Merge と merge lifecycle の自動化について理解する。
- feature の完全な変更と検証証拠を確認する。
- フィルタリング PR を作成する。
- 確認後にだけ Agent Merge を有効化し、PR がマージされたことを確認する。

## シナリオ

フィルタリング ワークフロー全体を通して、Copilot を使って機能の計画、実装、検証を行いました。Tailspin Toysでは、残りの PR 作業を自動化しつつ、マージ承認を開発者が管理したいと考えています。

## Agent Merge を紹介

**Agent Merge** は、pull request を取り込むために必要な作業を自動化します。有効化すると、Copilot は PR のブロック要因に対応します。失敗した continuous integration (CI) checks の修正、レビューコメントへの対応、必要な場合の rebase を行い、リポジトリで許可されている場合は GitHub auto-merge を有効化します。

ここまで、開発者自身が **Merge pull request** を選択してきました。Agent Merge にその責任を任せることもできます。作業を確認し、準備が整っていると判断してから Agent Merge を有効化してください。

## Agent Merge を使って PR を管理する

コードが作成されたので、変更をまとめて確認し、PR を作成し、Agent Merge に残りの処理を任せます。

1. Codespace に戻ります。
2. `/agent` を入力して agent ダイアログを開きます。
3. オプション一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って、新しい PR を作成します。
5. `/pr agentmerge` を使って agent merge を有効化します。
6. Copilot は PR の continuous integration プロセスを監視します。すべて成功すると、マージを実行します。
7. Copilotから「PR #14 was squash-merged successfully」のようなメッセージが表示されることを確認します。

> [!IMPORTANT]
> Agent Merge は、required approvals、branch protection、merge queues、リポジトリ設定、権限不足を回避しません。ブロックされた場合は、報告された理由を確認し、リポジトリで許可されている場合に限り、レビュー済みのマージを手動で完了してください。

## まとめと次のステップ

コード生成、テストとコード検証、そして pull request プロセスを含む開発プロセスの複数の部分を自動化できました。具体的には、次のことを行いました。

- Agent Merge と merge lifecycle の自動化について学びました。
- feature の完全な変更と検証証拠を確認しました。
- フィルタリング PR を作成しました。
- 確認後にだけ Agent Merge を有効化し、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、セッション共有、任意の cloud delegation に使える [Copilot CLI の slash commands をさらに確認][next-lesson]します。

## リソース

- [Copilot CLIで pull request を管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
