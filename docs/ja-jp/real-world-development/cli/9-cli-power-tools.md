---
title: "演習 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキスト管理、モデル選択、セッション共有、任意の cloud delegation に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

優れた CLI tool と同様に、GitHub Copilot CLI には操作用のスラッシュ コマンドが多数用意されています。これらのコマンドは高度な機能、内部情報、追加設定を提供します。すでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使用しました。次は、他の便利なコマンドを確認します。

この演習では、次のことを行います。

- `/context` と `/compact` を使って、Copilot の会話コンテキスト管理を確認する。
- `/model` を使って利用可能なモデルを確認する。
- `/share` がセッションをエクスポートまたは共有できることを理解する。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認する。

## シナリオ

CLI ワークフローの中核は完了しました。次は、コンテキスト管理、モデル切り替え、セッション共有、必要に応じた [Copilot cloud agent][about-cloud-agent]への委任など、いくつかの追加機能を確認します。

## Copilot CLI のコンテキストを確認する

規模が大きい、または複雑なタスクを扱う場合、モデルの最大コンテキスト ウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮し、スラッシュ コマンドで確認または手動圧縮できます。

1. Codespace に戻り、まだ開いていない場合はリポジトリのルートから Copilot CLI を起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、system instructions、tools、messages、free spaceによるコンテキストの割り当てを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. `/context` を再度入力し、結果を比較します。会話がすでに小規模な場合は大きな変化がないことがあります。

> [!NOTE]
> ウィンドウがいっぱいになると、Copilot CLI はコンテキストを自動的に圧縮します。タイミングを選びたい場合は `/compact` を使用します。無関係なタスクに切り替えて新しい会話を開始する場合は、`/clear` または `/new` を使用します。

## モデルを選択する

モデルごとに得意分野が異なり、開発者ごとに好みも異なります。Copilot CLIでは、使用するモデルを一覧表示して選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況を確認します。
3. 現在のモデルを維持するか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで作業し、学びを共有すると、AI tool の活用を改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートし、共有可能なリンクを作成し、GitHub gist に公開できます。

1. `/help` を入力し、インストールされているバージョンの `/share` オプションを確認します。
2. このセッションを共有する場合は、ローカルの Markdown エクスポートには `/share file` のように、目的に合った宛先を選択します。
3. 他の開発者に送信または公開する前に、エクスポートされた内容を確認します。セッション エクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクまたは gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## 任意: 並行作業または委任

このワークショップの中核は完了です。規模の大きなタスクに使えるコマンドも Copilot CLI が提供します。

- `/fleet` は、独立したサブタスクをサブ agent に分割し、並行実行できます。
- `/worktree` は、別タスク用の独立した Git worktree を作成できます。
- `/delegate` は、タスクを非同期で動作し pull request を作成する場合がある Copilot cloud agent に送れます。

これらのコマンドは任意です。追加の worktree やリモート作業が発生する場合があります。試す前に、スコープが明確で新しいタスクを開始し、通常のワークフローで結果を確認してください。非同期 agent 作業を詳しく確認する場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、設定、セッション共有、内部で起きていることの確認ができます。この演習では、次のことを行いました。

- `/context` と `/compact` を使って、Copilot の会話コンテキスト管理を確認しました。
- `/model` を使って利用可能なモデルを確認しました。
- `/share` がセッションをエクスポートまたは共有できることを学びました。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

利用できるスラッシュ コマンドは他にもあります。Copilot CLIで学べることはまだあります。旅の締めくくりとして、[学んだことを振り返り][next-lesson]、学習を続けるための次のステップを確認しましょう。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agentについて][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
