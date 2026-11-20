---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot クラウドエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot クラウドエージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** は、GitHub Copilot がクラウドで非同期に作業するための機能です。GitHub 上で作業を割り当てると、クラウドエージェントがバックグラウンドで作業を引き継ぎ、リポジトリの調査、変更の実施、pull request の作成を行うため、開発者はほかの作業に集中できます。

これらの演習では、クラウドエージェントが従うカスタム指示を追加し、GitHub Issue を割り当てて実装を依頼します。カスタムエージェントを確認して利用方法を調整し、エージェントダッシュボードからセッションを監視および操作し、最後に pull request を確認して結果を反復的に改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成 |
| [1. カスタム指示][ex1] | コンテキスト | クラウドエージェントが従うカスタム指示を追加 |
| [2. クラウドエージェント][ex2] | 非同期エージェント | Copilot クラウドエージェントに Issue を割り当て |
| [3. カスタムエージェント][ex3] | 専門エージェント | カスタムエージェントを確認して利用 |
| [4. エージェントの管理][ex4] | 監視 | エージェントセッションを監視および操作 |
| [5. 反復的な改善][ex5] | レビュー | pull request を確認し、Copilot の作業を反復的に改善して次の手順を選択 |

## 前提条件

このワークショップに参加する前に、次を確認してください。

- [ ] **Copilot Student、Pro、Pro+、Business、または Enterprise** プランが有効な GitHub アカウント
- [ ] ご自身のアカウントまたは Organization でクラウドエージェントが有効になっていること

> [!TIP]
> 有料プランをお持ちでない場合、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、MCP、コードレビュー、Copilot CLI 機能が含まれるため、すべてのハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> クラウドエージェントは **Copilot Student、Pro、Pro+、Business、Enterprise** の各プランで利用できますが、Copilot Free では利用できません。Copilot Business または Enterprise の場合は、管理者が Organization でクラウドエージェントを有効にする必要があります。

> [!NOTE]
> このハーネスでは MCP は扱いません。Copilot で MCP サーバーを使用する方法については、[CLI ハーネス](../cli/) または [VS Code ハーネス](../vscode/) を参照してください。

## はじめに

**[演習 0: 前提条件から開始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
