---
title: "演習 2 - GitHub Copilot クラウドエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に苦しむ組織はほとんどありません。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなかったためにバックログに残っている機能要望などが該当します。GitHub Copilot のクラウドエージェントは、コードの更新や機能追加などのタスクを自律的に実行できます。作業が完了すると、開発者が確認できるドラフト pull request を生成します。これにより、定型的な作業を任せられ、開発プロセスを加速し、開発者はより重要な課題に集中できます。

Copilot クラウドエージェントで次のことを確認します。

- コード生成環境をカスタマイズする方法
- 操作を安全に実行する方法
- Issue のスコープを明確にする重要性
- Issue を Copilot に割り当てる方法

## シナリオ

Tailspin Toys には、対処したい技術的負債があります。最初のバージョンのサイトを作成した契約社員は、ドキュメントの状態を最適とは言えないものにしており、実際にはほとんどドキュメントがありません。最初のステップとして、アプリケーションのすべてのエクスポートされた関数に TSDoc ドキュメントコメントを追加したいと考えています。

また、デザインチームはゲームの発見体験を改善する準備を進めています。各ゲームの詳細ページに関連するゲーム、つまり同じカテゴリのほかのタイトルを表示し、ユーザーがゲームの閲覧を続けられるようにしたいと考えています。現時点では洗練された実装は必要ありませんが、UX の受け入れテストに使用できるものが必要です。これは現在のブロッカーですが、優先度の高い Issue がほかにもあります。

どちらも優先度が下がりがちなタスクであり、Copilot クラウドエージェントに割り当てるのに適しています。Copilot クラウドエージェントは非同期に作業するため、開発者はほかのタスクに集中できます。完了後に Copilot の作業を確認し、すべてが想定通りか検証できます。

## GitHub Copilot クラウドエージェントの概要

[GitHub Copilot クラウドエージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、複数の方法で作業を依頼できます。割り当てられると、Copilot は進捗を追跡するドラフト pull request を作成し、環境をセットアップしてタスクの実行を開始します。セッションの進行中または完了後に Copilot の作業内容を確認できます。提案された解決策を確認できる状態になると、pull request で開発者に通知します。

## スコープを明確にする指示の重要性

GitHub Copilot を使用していても、考える必要がなくなるわけではありません。単純に見える操作でも、詳細を確認すると複雑になる場合があります。したがって、[Copilot クラウドエージェントに割り当てるタスクのスコープには注意][cloud-agent-best-practices]してください。AI ペアプログラマーとして扱い、段階的に作業し、学び、実験し、状況に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示とそのガイダンス方法を紹介しました。カスタム指示のハンズオンを完了している場合は、`.github/copilot-instructions.md` の動作を確認済みです。未完了の場合でも、ここで簡単に確認しておくとよいでしょう。Copilot クラウドエージェントに作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後からその影響を確認できるようにします。

リポジトリの GitHub Web UI または、すでに起動している Codespace のエディターで次のファイルを開きます。

- `.github/copilot-instructions.md` - TypeScript の規約や TSDoc ドキュメントコメントの要件など、**Code standards** セクションを確認します。
- `.github/instructions/unit-tests.instructions.md` - 指示の適用範囲を `**/*.test.ts` ファイルに設定する `applyTo` フロントマターを確認します。

次のセクションで `Code lacks documentation` Issue をクラウドエージェントに割り当てるときは、生成された pull request に含まれる TSDoc ドキュメントコメント、TypeScript の規約、コメントヘッダーを確認してください。これらは指示ファイルによって提供されます。後の演習で pull request を確認するときにも、この点を再度取り上げます。

## Copilot クラウドエージェントの開発環境をセットアップする

作成するコードの環境は、関係者にかかわらず、特定の環境とセットアップスクリプトが必要になることが多く、適切な状態を確認する必要があります。Copilot にタスクを割り当てる場合も同様です。Copilot は SWE と同じようにタスクを実行します。

クラウドエージェントは作業に [GitHub Actions][github-actions] を使用します。[特別なセットアップワークフロー][setup-workflow]を作成してこの環境をカスタマイズできます。作業開始前に実行されるワークフローは `.github/workflows/copilot-setup-steps.yml` ファイルに設定します。これにより、必要な開発ツールと依存関係にアクセスできます。このラボではワークフローが事前設定されているため、スムーズに進められます。Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、単一の Astro アプリ用に移行およびシード済みの SQLite データベースへアクセスできます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *実行するステップ*に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが付与されます。
    permissions:
      # セットアップ手順の一環としてリポジトリをクローンする場合（たとえば依存関係のインストール）は、`contents: read` 権限が必要です。セットアップ手順でリポジトリをクローンしない場合は、ステップの完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js（アプリ全体が Astro + Drizzle/libSQL に移行済み）
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

      # ビルドとテストで使用できるよう、ローカル SQLite データベースを移行およびシードします。
      - name: Set up the database
        run: npm run db:setup
```

このファイルはほかの GitHub ワークフローと同じ形式ですが、重要な点がいくつかあります。

- 単一のジョブ `copilot-setup-steps` が含まれています。Copilot が pull request の作業を開始する前に、このジョブは GitHub Actions で実行されます。
- リポジトリの **Actions** タブからワークフローを手動で実行できる `workflow_dispatch` トリガーを確認します。ワークフローが正常に実行されることを、Copilot の実行を待たずにテストできます。

## ドキュメントを追加する

ドキュメントの重要性は誰もが理解していますが、多くのプロジェクトでは情報が古くなっているか、ドキュメント自体がありません。これは対処されないことが多い技術的負債で、生産性を低下させ、コードベースの保守や新しい開発者の参加を困難にします。幸いなことに、Copilot はドキュメント作成に優れているため、この Issue を Copilot クラウドエージェントに割り当てるのに適しています。バックグラウンドで必要なドキュメントを生成します。後の演習で作業内容を確認します。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue ダイアログを開きます。
4. **Blank issue** を選択して、空の Issue を作成します。
5. **Title** に `Code lacks documentation` を設定します。
6. **Description** に次を設定します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

  ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` のようなタイトルが付いた、新しく生成された pull request (PR) を開きます。一覧に新しい PR が表示されない場合は、しばらく待ってからブラウザーウィンドウを更新します。
12. 数分後に、Copilot が todo リストを作成したことを確認できます。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかる場合があります。Copilot は環境をセットアップし（前述のワークフローを実行し）、プロジェクトを分析して、問題への最適なアプローチを判断しています。

13. リストと Copilot が実行するタスクを確認します。
14. pull request のタイムラインを下にスクロールし、Copilot が Issue の作業を開始した旨の更新が表示されることを確認します。
15. **View session** ボタンを選択します。

  ![Copilot セッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新されたインジケーターを表示するには、ウィンドウを更新する必要がある場合があります。

16. ライブセッションをスクロールして、Copilot がどのように問題を解決しているか確認します。コードを調査して状態を理解する方法、どのタイミングで処理を停止して計画を決定するか、コードを作成する方法などが含まれます。

作業には数分かかる場合があります。Copilot クラウドエージェントの主な目的の 1つは、非同期にタスクを実行できるため、開発者がほかの作業に集中できることです。この機能を活用し、ほかのタスクを Copilot クラウドエージェントに割り当てながら、アプリケーションに機能を追加するためのコード作成に取り組みます。

## ゲーム詳細ページに関連ゲームセクションを追加する

前述のように、Copilot クラウドエージェントを使用すると作業を分担できるという利点があります。デザインチーム向けに関連ゲームセクションを追加する作業はそれほど時間がかからない場合がありますが、その時間をほかのタスクに使用できるかもしれません。Copilot クラウドエージェントに割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue ダイアログを開きます。
4. **Blank issue** を選択して空のテンプレートを使用します。
5. **Title** に `Show related games on the game details page` を設定します。
6. **Description** に次を設定します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

しばらくすると、Issue の最初のコメントに Copilot が作業中であることを示す 👀 が表示されます。

![Copilot が Issue の作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2つ目のリクエストに取り組んでいます。Copilot クラウドエージェントは SWE と同様に動作するため、積極的に監視する必要はありません。完了後に確認できます。ここではカスタムエージェントの作成と使用に進みます。

## 概要と次のステップ

このレッスンでは、[GitHub Copilot クラウドエージェント][copilot-agents]について確認しました。クラウドエージェントを使用すると、Issue を Copilot に割り当て、技術的負債への対応、新機能の作成、コードの別フレームワークへの移行支援などを非同期で実行できます。

次の概念を確認しました。

- コード生成環境をカスタマイズする方法
- 操作を安全に実行する方法
- Issue のスコープを明確にする重要性
- Issue を Copilot に割り当てる方法

クラウドエージェントがバックグラウンドで作業を進めている間、カスタムエージェントの作成と使用に進みます。[Copilot クラウドエージェントでも MCP サーバーを使用でき][cloud-agent-mcp]、カスタム指示を利用できます。これらは前のモジュールで確認しました。

## リソース

- [Copilot クラウドエージェントについて][copilot-agents]
- [Copilot に GitHub Issue を割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
