---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成済みのアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を体験します。反復的に作業を進め、変更を計画し、コードを作成し、動作をテストし、pull request を確認し、agent を使って作業を管理します。

## シナリオ

ボードゲームのクラウドファンディングを提供する架空の企業 Tailspin Toys の新しい開発者として作業します。チームのバックログは GitHub issue として登録済みで、開発者が取り組む準備ができています。フィルタリングやページネーションなどの機能開発に加えて、アクセシビリティやコーディング標準などの品質改善も含まれています。

## 環境を選択する

GitHub Copilot は、作業環境に応じて利用できます。作成方法に合った環境を選択してください。各ワークショップは独自のセットアップで始まり、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルで agent 型アシスタントを使用します。Plan モード、Autopilot モード、instructions、skills、custom agents、Playwright MCP、Agent Merge、実用的な slash commands を扱います。

### [GitHub Copilot app][app]

複数の agent セッションを並行して実行し、セッション モードを切り替え、canvas で共同作業し、デスクトップ app から GitHub issue と pull request を管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub issue を非同期 agent に割り当て、作業を案内し、進捗を監視し、agent が作成する pull request を確認します。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使用します。エディターを離れずに Copilot Chat の agent mode、MCP サーバー、custom agents を扱います。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
