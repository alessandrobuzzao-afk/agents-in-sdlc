---
title: "演習 6 - Playwright MCP を使って機能を検証する"
description: "Copilot CLI の Playwright MCP を確認または設定し、ブラウザーでフィルタリング機能を操作します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

すでに説明したとおり、コードを書くこと以外にも多くの作業があります。データや外部サービスを扱う必要があり、Copilot が利用できる自動化を追加する必要もあります。そのために MCP サーバーを使用します。MCP サーバーを使うと、Copilot は CLI に組み込まれた機能だけでなく、外部のツールやサービスを利用できるようになります。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI における MCP の使われ方を理解する。
- Playwright MCP サーバーが利用できない場合は追加する。
- agent にブラウザーを操作させ、フィルタリング機能を確認するよう依頼する。

## シナリオ

単体テストと end-to-end テストは重要ですが、UI の更新を検証するには実際に UI を操作する必要があります。Copilot が利用者と同じようにサイトを操作できるようにすることで、変更の自動化を強化し、更新が期待どおりに動作するという確信を高めます。

## Model Context Protocol (MCP)とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI agent が外部ツールやサービスと通信する方法を提供します。MCP を使用すると、AI agent は外部ツールやサービスとリアルタイムで通信できます。そのため、最新の情報にアクセスし、開発者に代わって操作を実行できます。

これらのツールとリソースは、AI agent と外部ツールやサービスを橋渡しする MCP サーバーを通じて利用します。各 MCP サーバーは、AI agent が利用できる一連のツールとリソースを表します。

代表的な既存 MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、pull request を管理する APIへのアクセスを提供します。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

利用できる MCP サーバーは多数あります。GitHub の [MCP registry][mcp-registry] では、エコシステムの発見と貢献を支援しています。

> [!CAUTION]
> MCP サーバーは、プロジェクト内の他の依存関係と同様に扱ってください。使用前にソースコードを確認し、公開元を検証し、セキュリティ上の影響を考慮してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーが利用できない場合は追加し、Copilot が利用者と同じ方法で website を操作できるようにします。

> [!NOTE]
> Copilot CLIですでに Playwright MCP が設定されている場合は、セットアップ手順をスキップして次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLIで次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. name に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバータイプとして **STDIO** を確認し、<kbd>Tab</kbd> を押します。
5. 次の内容を **Command** ダイアログに貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを終了します。

## Playwright を使って Copilot に機能を確認させる

これまで機能が期待どおり動作することを手動で確認しました。次は、Copilot で利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを使って、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は MCP サーバーを使用するよう明示的に依頼する必要はありません。Copilot が判断して使用します。ただし、使用すべきツールを開発者側で把握している場合は案内しておくと、より一貫した結果になり、トークンを節約できます。

2. Copilot がブラウザーで実行する各手順を確認し、機能が動作するか検証します。
3. レポートを読み、すべてが期待どおり動作することを確認します。

Copilot はサーバーを起動し、Playwright を使って website を操作し、サーバーを停止してレポートを返します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使用し、実際のブラウザーで機能を確認できました。ここでは、次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLIにおける MCP の使われ方を学びました。
- Playwright MCP サーバーが利用できない場合は追加しました。
- agent にブラウザーを操作させ、フィルタリング機能を確認してもらいました。

次は、skill とブラウザー ツールを専門的な role に統合する [QA custom agent を作成][next-lesson]します。

## リソース

- [MCPとは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
