---
title: "レッスン 1 - GitHub Copilot CLI のインストール"
description: "Codespace に Copilot CLI をインストールして認証し、環境を確認して、用意されたフィルター Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディングアシスタントです。コードベースの調査、コードの生成、コマンドの実行、外部ツールとの連携をコマンドラインから行えます。タスクを任せ、変更を依頼しながら集中して作業できます。最初のステップはツールをインストールすることです。幸い、使い慣れたツールを使って行えます。

このレッスンでは、次の内容を学習します。

- npm を使用して GitHub Copilot CLI をインストールする。
- GitHub アカウントで認証する。
- ワークショップのリポジトリを信頼し、簡単な会話を試す。
- 統合 GitHub MCP サーバーを使用してフィルター Issue を見つける。

## シナリオ

チームは、増え続けるバックログに対応するために AI エージェントの利用を始めています。Copilot CLI は、その機能を多くの開発者が日常的に使用するターミナルに提供します。このレッスンでは、インストールと認証を完了し、ワークショップの残りで使用できる状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、npm を使用します。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、バージョン要件を満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されるはずです。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されるはずです。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 求められたら、デバイスコードの手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. パスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して yes と回答します。

> [!NOTE]
> Codespace では、GitHub セッションからすでに認証されている場合があります。認証を求めずに Copilot CLI が起動した場合は、そのまま進めます。

## 使用方法を確認する

通常のシェルプロンプトで入力したコマンドは Codespace で直接実行されます。Copilot CLI を起動すると、自然言語はエージェントに送られ、スラッシュコマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストールされたバージョンで利用できるコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. Copilot に簡単な質問をして、動作を確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 回答を読み、回答前に Copilot がリポジトリを調査する様子に注目します。
5. `/mcp list` を入力し、組み込み GitHub MCP サーバーが利用可能であることを確認します。
6. Copilot にフィルター Issue の検索を依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて Issue を読みます。簡単な最初の変更を完了した後に使用します。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチで動作し、worktree を自動的には作成しません。変更ごとにフィーチャーブランチを作成します。

## ワークショップのショートカットを使用する

Copilot CLI は通常、既定の権限外でツールを使用する前に確認します。このワークショップでは、`--yolo` を使って再起動します。これはユーザー承認済みのショートカットで、Codespace 内の承認プロンプトを削除して演習に集中できるようにします。

> [!CAUTION]
> `--yolo` は完全な自動権限 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を有効にします。Codespace または VM のような分離された環境でのみ使用し、日常的な開発では既定のエイリアスとして設定しないでください。詳細は [ツールの使用を許可または拒否する][allow-all-warning]を参照してください。

このワークショップでは、後続のレッスンで Issue と pull request を操作するために使用する読み取り / 書き込み GitHub MCP ツールを `--enable-all-github-mcp-tools` が有効にします。Codespace はローカルコンピューターへのアクセスを制限しますが、認証済みの GitHub リソースは実環境です。公開またはマージする前に変更をレビューしてください。

1. `/exit` を使って Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについてもう1つ簡単な質問して会話が動作することを確認し、`/exit` で終了します。

Copilot は会話を自動的に保存します。後で指示を変更したりエージェントを追加したりした場合は、`copilot --resume` を使用して同じ機能に関する会話とブランチに戻ります。

## まとめと次のステップ

おめでとうございます。このレッスンでは、次の作業を行いました。

- npm を使用して GitHub Copilot CLI をインストールした。
- GitHub アカウントで認証した。
- ワークショップのリポジトリを信頼し、簡単な会話を試した。
- 統合 GitHub MCP サーバーを使用してフィルター Issue を見つけた。

次は、[最初の対象を絞った変更を開始し][next-lesson]、Copilot CLI を使ってゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
