---
title: "演習 2 - 星評価を追加する: 小さな成功"
description: "Copilot CLI を使ってゲームカードを少し変更し、転送されたブラウザーで確認して、最初の pull request としてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したので、次はプロジェクトに初めて変更を加えます。小規模な変更に留めます。ゲームにはすでにデータとして星評価が設定されていますが、ホーム ページのゲームカードには表示されていません。agent に表示を追加してもらい、変更を確認し、最初の pull request としてマージします。

この演習では、次のことを行います。

- 機能ブランチで焦点を絞った Copilot の会話を開始する。
- agent にプロジェクトへの小規模な変更を依頼する。
- `/diff` を使って変更を確認する。
- app を起動し、転送されたブラウザーで変更を確認する。
- 最初の pull request を作成してマージする。

## シナリオ

Tailspin Toys の各ゲームには星評価を設定でき、ゲームの詳細ページにはすでに表示されています。ただし、ホーム ページのゲームカードには、タイトル、カテゴリ、パブリッシャー、説明しか表示されていません。ウォームアップとして、既存の評価を各カードに表示させます。これは自己完結した小さな変更で、最初のセッションに最適です。

## 会話の基本

**conversation** は、Copilot CLI を使ってタスクを進める場です。Copilot app と異なり、通常の CLI conversation は専用の worktree を作成せず、ターミナルで現在チェックアウトされているリポジトリと Git ブランチを使用します。保存された会話に戻って同じ議論を続けることもできますが、ファイルとブランチはディスク上の通常の Git 状態として残ります。

会話内では、プロンプトと agent の応答、プロジェクトを調査してファイルを編集する agent のツール操作、`/diff` で確認できる変更という、3つの要素を確認できます。

## 会話を開始して変更を依頼する

新しい会話を開始し、機能の実装に取りかかります。

1. Codespace に戻ります。
2. 前回からターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）を押します。
3. 開いていない場合は、次のコマンドで Copilot を起動します。

   ```bash
   copilot --yolo
   ```

4. `/new` slash コマンドを使い、<kbd>Enter</kbd> を押して新しいセッションを開始します。
5. 次のプロンプトを使って変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調査し、ゲームの詳細を表示するファイルを特定し、必要なコードを作成します。これで Copilot CLI による新機能を追加できました。

## 差分を確認する

小規模な変更でも、生成された AI の変更はすべてマージ前に確認する必要があります。ここで Copilot CLI 内で変更内容を確認します。

1. `/diff` を入力して、変更されたファイルをすべて確認します。
2. 評価がある場合は数値がゲームカードに表示され、`starRating` が `null` の場合は `No rating yet` が表示されることを確認します。
3. テストが両方の状態をカバーしていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. 確認が完了したら、<kbd>Esc</kbd> を押して diff screen を終了します。

> [!NOTE]
> Copilot を含む生成 AI ツールは確率的で決定論的ではないため、生成されるコードは完全には同じにならない場合があります。特定の実装を期待するのではなく、動作を確認してください。

## 変更を確認する

コードを読むだけで動作するとは仮定できません。Copilot に website を起動するよう依頼し、Codespaces が転送するブラウザーで更新された user interface (UI) を確認します。

1. app を起動するよう Copilot に依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces がポート `4321` が利用可能になったと通知したら、**Open in Browser** を選択します。
3. ゲームカードに5段階中の評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーの停止を依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初の pull request を作成してマージする

機能を作成できました。新しいコードをプロジェクトにマージするため、pull request (PR) を作成します。

1. default agent に変更のコミットを依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は既存のコミットを push し、PR の URL を表示します。
3. <kbd>Command</kbd>（Mac）または <kbd>Ctrl</kbd>（Windows/Linux）を押しながら、Copilot CLI に表示された URL を選択して PR を開きます。
4. 変更されたファイルと checks を確認します。
5. 準備ができたら、**Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` を使って Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って、最初の変更を実装できました。具体的には、次のことを行いました。

- 機能ブランチで焦点を絞った Copilot の会話を開始しました。
- agent にゲームカードへの小規模な変更を依頼しました。
- `/diff` を使って変更を確認しました。
- app を起動し、転送されたブラウザーで星評価を確認しました。
- 最初の pull request を作成してマージしました。

次は、[フィルタリング issue から開始し、Plan モードと Autopilot モードを使用][next-lesson]して、規模の大きな機能を構築します。

## リソース

- [GitHub Copilot CLIについて][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
