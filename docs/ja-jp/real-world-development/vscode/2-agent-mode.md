---
title: "演習 2 - Copilot Agent Mode を使って新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションに対する最も簡単な更新でも、通常は複数のファイルを更新し、テストの実行などの操作が必要です。開発者の作業では、必要なファイルをすべて特定し、変更を加え、テストを実行し、デバッグし、変更漏れのファイルを見つけて、さらに更新します。作業はそれだけでは終わりません。

ここで Copilot Agent Mode が役立ちます。

Copilot Agent Mode は、IDE 内でより自律的に動作するよう設計されています。開発者に似た動作をします。まず既存のプロジェクト構造を調査し、必要な更新を実施し、テストなどのタスクを実行し、検出した問題を自動的に修正します。この演習では、Agent Mode を使ってサイトに新機能を追加する方法を確認します。

> [!NOTE]
> 名前は似ていますが、Agent Mode とクラウドエージェントは異なる種類の操作体験向けに構築されています。Agent Mode は IDE 内でタスクを実行し、迅速なフィードバックと対話を可能にします。クラウドエージェントは非同期のペアプログラマーとして、チームメンバーのように Issue やプルリクエストを介して対話しながら作業します。

この演習では、次の方法を確認します。

- Copilot Agent Mode がプロジェクトを調査し、関連ファイルを特定し、連携した変更を行う方法。
- GitHub Copilot Agent Mode が UI とデータレイヤーの両方にわたって新機能を実装する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

## シナリオ

ゲーム数が増えるにつれ、ユーザーがカテゴリとパブリッシャーで絞り込めるようにする必要があります。前の演習でパブリッシャー用ヘルパーを追加しているため、ここでは残りのデータレイヤー、UI、テストの作業を Copilot Agent Mode で完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを調べて現在の機能を理解しましょう。

この Web サイトは、開発者向けテーマを使用したボードゲームのクラウドファンディングプラットフォームです。ユーザーがゲームを掲載し、ゲームの詳細を表示できます。単一の Astro アプリで、ビルド時に各ページを静的 HTML としてレンダリングします。ページはフロントマター内で Drizzle ORM を介してローカル SQLite データベースを直接照会し、別のバックエンド API やクライアント側 UI フレームワークは使用しません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、対話機能は標準 DOM API を使用した小規模かつスコープ限定の Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトの起動を簡単にするため、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。次の手順で GitHub Codespace で実行できます。

1. codespace に戻ります。前の演習の後は `main` ブランチに戻っています。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナルウィンドウを開きます。
3. フィルター機能の作業用に新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログボックスが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルで Astro サーバーのアドレス `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> codespace を使用している場合は、Codespace ターミナルから localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは現在 Web サーバーをホストしている codespace に接続するプライベートトンネルです。

### Web サイトを確認する

Web サイトの起動後、その機能を確認できます。主な機能は次のとおりです。

- **Home Page**: ボードゲームの一覧をタイトル、画像、説明とともに表示します。
- **Game Details Page**: ゲームを選択すると、そのタイトル、説明、パブリッシャー、カテゴリなどの詳細情報を表示するページに移動します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を開始します。モードとモデルの選択は演習ごとに異なるため、各ステップの指示に従ってください。

まず、テンプレートからプロジェクトをセットアップしたときに自動作成された issue のバックログを確認するため、GitHub Copilot に一覧の表示を依頼します。

1. Chat ビューのエージェント選択メニューから **Agent** を選択します。**Agent** エージェントは、複数ファイルにわたる変更を計画・実装し、ターミナルコマンドを実行し、ツールを呼び出します。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨しているわけではありません。このワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順に含めています。良い操作体験が得られるよう、コード提案が比較的一貫した形になることを期待しています。ただし、大規模言語モデル (LLM) は確率的であるため、実際の提案がワークショップに示された内容と異なる場合があります。これは正常で予想される動作です。

> [!NOTE]
> LLM は確率的なため、Copilot が別の MCP コマンドを使用する場合があります。ただし、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信し、issue のバックログについて確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot が、ユーザーにとって最も役立つ項目を基準に、項目にも優先順位を付けています。

## 指示ファイルを確認する

エージェントにコード生成を開始させる前に、Copilot に作業コンテキストを提供する指示ファイルを確認しておくとよいでしょう。Web サイトへの機能追加をどのように進めるかについてのコンテキストを含む[ユーザーインターフェイス (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)ファイルを使用します。

1. codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加への全体的なアプローチに関するガイドを確認します。確認項目には次が含まれます。
   - アーキテクチャの概要。
   - コンポーネント設計、テスト可能性、アクセシビリティに関する原則。
   - 各ファイル形式向けの指示ファイルへのリンク。
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指示ファイルでは、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所を基準に指定します。これにより、再利用が可能になり、複雑な指示を小さな管理しやすい単位に分割し、例やテンプレートを提供できます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくともアプリケーションに 3 件の更新を行う必要があります。

- データレイヤー (`src/lib/`) の残りのフィルター処理を追加または改善する
- フィルター動作のテストを追加または更新する
- ゲーム一覧ページを更新してフィルター UI を導入する

さらに、コードベースにマージする前にテストを実行し、すべて成功させる必要があります。Copilot Agent Mode はこれらのタスクを実行できます。機能を追加しましょう。

1. 現在の Copilot の会話を続けても、**New Chat** を選択して会話を開始し直してもかまいません。
2. **Add Context**、**Instructions**、**ui** を選択し、指示ファイルを追加します。

   ![UI 指示ファイルの選択例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント選択メニューで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログの関連 issue に基づいて機能を実装するよう、次のプロンプトを Copilot に送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトの調査を開始し、目的の機能に関連するファイルを特定します。データレイヤー用ヘルパー、UI、テストが見つかる様子を確認してください。その後、ファイルの変更とテストの実行を開始します。

   ![Copilot がプロジェクトファイルを調査しているところを示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot は、プロジェクトの調査、ファイルの変更、テストの実行など、複数のタスクを実行します。タスクとコードベースの複雑さによっては数分かかる場合があります。その間、コードエディターに **Keep** ボタンと **Undo** ボタンが表示されることがあります。Copilot の作業が完了すると、すべての変更について **Keep** または **Undo** を選択できます。作業中はボタンを選択する必要はありません。

7. Copilot のプロンプトに従い、**Continue** を選択してテストを実行します。

   ![テストの実行許可を求める Copilot Chat ペインのダイアログを示すスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に処理が一時停止したり、一部のテストが失敗したりする場合があります。これは問題ありません。Copilot はコード生成とテストを行き来しながら作業を進め、タスクを完了してエラーが検出されなくなるまで処理を続けます。

   ![Copilot Agent Mode の完全なチャットセッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 潜在的な問題がないか、生成されたコードを確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは、必ず確認することが重要です。

10. Web サイトを実行中のブラウザーに戻ります。新しい機能を確認します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

おめでとうございます。この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに新機能を追加する方法を確認しました。次の内容を確認しました。

- GitHub Copilot Agent Mode が UI とデータレイヤー全体にわたって新機能を実装できること。
- Copilot Agent Mode がプロジェクトを調査し、関連ファイルを特定し、連携した変更を行えること。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストを確認すること。

次に、[Playwright MCP サーバーで機能をテスト][next-lesson]し、プルリクエストを作成しましょう。

### ボーナス探索演習 - ページネーションを実装する

ゲーム数が増えるにつれ、ページネーションが必要になります。この演習で学んだスキルを使って、サイトにページネーションを実装するよう Copilot に依頼してください。コードに関する検討事項は次のとおりです。

- 既存の指示ファイルを使用するなど、既存のベストプラクティスに従ってください。
- ページサイズをユーザーが選択できるようにするかハードコードするかなど、ページネーションの実装方法を検討してください。
- プロンプトを作成する際は、実現したい実装を Copilot に伝えるために必要なガイダンスを提供してください。
- 必要に応じて、変更を依頼したりコンテキストを追加したりして、GitHub Copilot と反復作業を行う場合があります。これは Copilot を使った作業の通常の流れです。

## リソース

- [Copilot のアスク、編集、エージェントモード: 用途と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
