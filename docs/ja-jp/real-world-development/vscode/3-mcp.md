---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルター機能を実装しました。プルリクエストを作成する前に、ブラウザーで実際に動作することを確認しましょう。アプリを自分で操作する代わりに、**Playwright MCP サーバー**に接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次の作業を行います。

- Model Context Protocol (MCP) と、MCP サーバーによる Copilot のツール拡張について確認します。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にブラウザーからフィルター機能を手動テストさせます。
- ブランチを公開し、フィルター機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部にアクセスできると、さらに強力になります。Model Context Protocol (MCP) は Copilot がそれを実現する仕組みで、エージェントと外部ツールやサービスを接続する標準的な方法です。

![エージェントモードの内部構造と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)は、AI エージェントが外部ツールやサービスと通信するための手段を提供します。MCP を使用すると、AI エージェントはリアルタイムで外部ツールやサービスと通信できます。リソースを使って最新情報にアクセスし、ツールを使って利用者に代わって操作を実行できます。

これらのツールとリソースは MCP サーバー経由で利用します。MCP サーバーは AI エージェントと外部ツールやサービスの間のブリッジとして機能します。MCP サーバーは、既存 API や NPM パッケージなどのローカルツールを含む外部ツールとの通信を管理します。各 MCP サーバーは、AI エージェントが利用できる異なるツールやリソースの集合を表します。

よく利用される既存の MCP サーバーには、次のようなものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理するための API セットへのアクセスを提供します。AI エージェントが、新しいリポジトリの作成、既存リポジトリの更新、issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使用したブラウザー自動化機能を提供します。AI エージェントが Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

ほかにもさまざまなツールやリソースへのアクセスを提供する MCP サーバーがあります。エコシステムの検出性と貢献を高めるため、GitHub は [MCP レジストリ](https://github.com/mcp)を提供しています。

> [!CAUTION]
> セキュリティに関しては、MCP サーバーをプロジェクトのほかの依存関係と同じように扱ってください。MCP サーバーを使用する前に、ソースコードを注意深く確認し、公開元を検証し、セキュリティ上の影響を検討してください。信頼できる MCP サーバーだけを使用し、機密リソースや操作へのアクセス権の付与には注意してください。

## MCP 構成を確認する

`.vscode/mcp.json` ファイルには、この VS Code ワークスペースで利用可能な MCP サーバーが構成されています。

1. codespace で `.vscode/mcp.json` を開きます。
2. 次のように、すでに `github` サーバーが構成されているはずです。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクトテンプレートに含まれているため、前の演習で Copilot は issue のバックログを読み取れました。これは[リモート GitHub MCP サーバー][remote-github-mcp-server]を使用するため、ローカルにインストールする必要はありません。VS Code が HTTP 経由で接続し、Copilot がツールを初めて使用する際に GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 番目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot が制御できるブラウザーを提供するため、機能テストに必要です。

1. `.vscode/mcp.json` に、`github` と並んで `playwright` エントリを追加します。ファイルは次のようになります。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル**サーバーです。VS Code は `npx @playwright/mcp@latest` を実行してこのサーバーを起動します。`--headless` フラグは、ブラウザーを表示ウィンドウなしで実行するよう Playwright に指示します。codespace には表示用デスクトップがないため、これは必須です。

> [!NOTE]
> Tailspin Toys プロジェクトでは、エンドツーエンドテストにすでに Playwright を使用しているため、Playwright に必要なブラウザーがインストールされていることが多いです。Copilot がブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行して再試行してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**オンデマンド**で起動します。Copilot がサーバーのツールを初めて必要としたとき、VS Code がサーバーを起動し、信頼を確認するよう求めます。

1. `.vscode/mcp.json` の `playwright` エントリ上部に **Start** 操作が表示されます。選択してサーバーを今すぐ起動し、構成が正しいことを確認します。
2. サーバーを信頼するかどうかを VS Code が確認した場合は、構成を確認して信頼を選択し、サーバーを起動できるようにします。

手動での起動は任意です。これを省略した場合は、次のステップで Copilot がツールを初めて必要としたときに、VS Code がサーバーを起動し、信頼を確認するよう求めます。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できる実際のブラウザーを提供します。あなたがアプリを操作して動作を確認する代わりに、エージェントがページを開いて移動し、フィルターを適用し、その結果をチャットに返して要約できます。会話を離れずに、機能が期待どおり動作することを確認する最も迅速な方法です。

内部では、Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を基に動作します。つまり、支援技術と同じように、ボタン、リンク、リスト項目などの構造化されたラベル付き要素を基にエージェントが判断します。そのため、簡単な機能確認が軽いアクセシビリティの健全性チェックにもなります。

サーバーを接続し、アプリを実行したら、Copilot に実装したばかりのフィルター機能を操作させます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバー経由でブラウザーを起動し、各ステップを操作し、結果を報告します。issue の受け入れ基準と照らして要約を確認します。問題がある場合はフォローアップの質問を送るか、プルリクエストを作成する前に修正を依頼します。

> [!NOTE]
> このテストでは、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送信する前に再起動します。Playwright MCP サーバーを Copilot が初めて使用するときは、ブラウザーのダウンロードが必要になる場合があります。ブラウザーが見つからないと報告された場合は、`npx playwright install chromium` を実行して再試行してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が正常に動作することを確認できたので、チームによるレビューを受けるためプルリクエストを作成する準備が整いました。最初のステップは、`filtering-vscode` ブランチを公開することです。

1. codespace の **Source Control** パネルを開き、Copilot による変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使用してコミットメッセージを生成します。

   ![変更内容を示す Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

プルリクエストの作成方法はいくつかあります。github.com または GitHub コマンドラインインターフェイス (CLI) を使用できます。ただし、すでに GitHub Copilot を使って作業しているため、Copilot に PR の作成を任せましょう。関連する issue を見つけ、その issue に関連付けた PR を作成できます。

1. Copilot Chat パネルを開き、**New Chat** を選択して新しいセッションを開始します。
2. エージェント選択メニューで **Agent** が選択されていることを確認し、Copilot が GitHub ツールを使用できるようにします。
3. Copilot に PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、Copilot に情報収集や操作の実行を許可します。GitHub ツールを初めて使用するときは、GitHub 認証を求められる場合があります。表示される手順に従って許可します。
5. Copilot が issue を検索し、正しい issue を見つけて PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択してプルリクエストを確認します。まだ**マージしないでください**。

## まとめと次のステップ

おめでとうございます。この演習では、次の作業を行いました。

- Model Context Protocol (MCP) と、MCP サーバーによる Copilot の新しいツール拡張について確認しました。
- Playwright MCP サーバーをワークスペースに追加しました。
- リリース前にブラウザーでフィルター機能を手動テストするために使用しました。
- ブランチを公開し、フィルター機能のプルリクエストを作成しました。

MCP サーバーを使って機能を*テスト*しました。MCP は Copilot を拡張する 1つの方法にすぎません。次は、アクセシビリティレビューなどの焦点を絞ったタスクを効率化する[カスタムエージェントを作成][next-lesson]しましょう。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
