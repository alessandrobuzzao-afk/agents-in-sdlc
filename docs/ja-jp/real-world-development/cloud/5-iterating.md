---
title: "演習 5: GitHub Copilot の作業を反復的に改善"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業内容を確認する

このラボ全体で、GitHub Copilot に、ユーザーエクスペリエンスの改善と機能追加を目的とした複数のタスクを依頼しました。コードへのドキュメントの追加、デザインチームが反復改善できる関連ゲーム機能の構築、ハイコントラストモードとライトモードのトグルを含むアクセシビリティ機能の実装を依頼しました。コード変更を確認し、必要に応じて Copilot にフィードバックを提供して作業を改善します。

### シナリオ

これまで何度も説明したように、ソフトウェア設計と DevOps の基本は、生成 AI が導入されても変わりません。生成されたコードは常に確認し、通常の DevOps プロセスで作業を進める必要があります。そのため、チーム全体にレビューを有効にする前に、GitHub Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能に関する提案を確認します。

## セキュリティと GitHub Copilot クラウドエージェント

Copilot クラウドエージェントは監督なしで非同期にタスクを実行するため、安全性を確保するためのセキュリティ制約があります。その一部は次のとおりです。

- Copilot はリポジトリへの読み取り専用アクセス権を持ち、書き込みアクセス権を持つのはコードに使用するブランチのみです。
- クラウドエージェントは GitHub Actions 内で、作業用の個別の一時環境を作成して実行します。
- GitHub Actions ワークフローを実行するには、すべて人間による承認が必要です。
- 外部リソースへの[アクセスはデフォルトで制限][agent-firewall]され、MCP サーバーも対象になります。

## 生成されたドキュメントを確認する

まず、GitHub Copilot クラウドエージェントが生成した最初の pull request (PR)、コードへのドキュメント追加を確認します。この作業は GitHub.com の標準的な PR インターフェイスで行います。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールによってブロックされたという警告が表示される場合があります。Copilot はデフォルトで外部リソースへのアクセスが制限され、外部 MCP サーバーへの呼び出しも対象となるため、これは**想定された動作**です。必要に応じて、[Copilot クラウドエージェントのファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して pull request の一覧を開きます。
3. **Add missing documentation** またはそれに近い、より具体的なタイトルが付いた pull request を開きます。

> [!NOTE]
> Copilot がまだタスクを実行中の場合、pull request に **[WIP]** フラグが表示されます。その場合は、Copilot が作業を完了するまで待ちます。完了まで数分かかることがあるため、その間は休憩するか、これまで学んだ内容を振り返ることができます。

4. pull request の準備ができたら、**Files changed** タブを選択して変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく作成された TSDoc ドキュメントコメントやその他のドキュメントを含む、更新されたコードを確認します。正確な変更内容は、セッションごとに異なります。

    コードを確認する際は、TSDoc ドキュメントコメント、TypeScript の規約、コメントヘッダーを探します。これらは演習 2 の開始時に確認したカスタム指示ファイルに基づいています。

6. 更新内容を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. 一部のワークフローが承認待ちになっていることを示すインジケーターが表示されます。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローの承認と実行](../../../_images/shared-approve-workflows.png)

9. pull request の Checks セクションでワークフローがキューに登録されます。問題がなければ、単一の Astro アプリに関するプロジェクトチェックが完了します。完了まで数分かかる場合があります。

## GitHub Copilot に変更をリクエストする

pull request 上で Copilot と連携する作業は、一方的なものではありません。チームのほかのメンバーにコメントする場合と同じように、pull request 内またはコードのインラインコメントで Copilot をタグ付けできます。Copilot はコメントを検出し、新しいセッションを開始して対応します。結果は非決定的であるため、依頼するテキストを固定的に示すことはできません。Copilot に更新を依頼する例としては、次のものがあります。

- 各コードファイルの先頭に、そのファイルの目的を簡潔に説明するコメントヘッダーを追加する。
- TypeScript ファイルと Astro ファイルに TSDoc ドキュメントコメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 生成されたドキュメントへの変更を依頼するコメントを追加し、ほかのユーザーと同じように **@copilot** をタグ付けします。上記の例を使用するか、コードベースに含めたいドキュメントに関するほかの提案を指定します。
2. **View Session** を選択して、Copilot が作業を実行する様子を確認します。Copilot が更新を行うために新しいセッションを開始することを確認します。
3. **Back to pull request** を選択して pull request に戻ることができます。

    ![pull request に戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、pull request に新しいコミットが表示されます。
5. **Files changed** タブを選択して変更を確認します。

満足するまで自由に反復改善できます。満足できたら、PR をドラフトからレビュー可能な状態に変更し、main ブランチにマージします。

![PR をレビュー可能な状態に変更](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能を確認する

ゲーム詳細ページで関連ゲームを表示する Issue の解決に向けて Copilot が生成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** またはそれに近い、より具体的なタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 更新内容を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. 一部のワークフローが承認待ちになっていることを示すインジケーターが表示されます。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローの承認と実行](../../../_images/shared-approve-workflows.png)

8. pull request の Checks セクションでワークフローがキューに登録されます。問題がなければ、単一の Astro アプリに関するプロジェクトチェックが完了します。完了まで数分かかる場合があります。
9. **オプション:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動し、ターミナルを開いて次のコマンドを実行します（`<branch-name>` は Copilot が作成したブランチ名に置き換えます。たとえば **copilot/fix-8** です）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を構築しました。前回と同様に、Copilot クラウドエージェントと反復的に連携して更新を依頼できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーと TSDoc ドキュメントコメントを追加したりするよう依頼できます（これはカスタム指示を更新する**前**に割り当てたタスクだったことを思い出してください）。前回と同様に、**Conversation** タブに新しいコメントを追加して依頼すると、Copilot は新しいセッションを開始します。

## アクセシビリティ機能を確認する

最後に、カスタムアクセシビリティエージェントを使用して実装されたアクセシビリティ機能を確認します。この PR には、演習 3で割り当てたハイコントラストモードと、演習 4で Mission control からリクエストしたライトモードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** またはそれに近い、より具体的なタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだタスクを実行中の場合、pull request に **[WIP]** フラグが表示されます。その場合は、Copilot が作業を完了するまで待ちます。完了まで数分かかることがあります。

4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 実装を確認し、特に次の点に注意します。
   - モードを切り替えるトグル UI コンポーネント
   - ユーザー設定を保持するための local storage の使用
   - ハイコントラストモードとライトモードの CSS またはスタイル変更
   - アクセシビリティ属性（ARIA ラベル、キーボードナビゲーションなど）
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 更新内容を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. 一部のワークフローが承認待ちになっていることを示すインジケーターが表示されます。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローの承認と実行](../../../_images/shared-approve-workflows.png)

9. pull request の Checks セクションでワークフローがキューに登録されます。問題がなければ、単一の Astro アプリに関するプロジェクトチェックが完了します。完了まで数分かかる場合があります。
10. **オプション:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動し、ターミナルを開いて次のコマンドを実行します（`<branch-name>` は Copilot が作成したブランチ名に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードのトグルをテストして、想定通りに動作し、ページの再読み込み後も設定が保持されることを確認します。

カスタムアクセシビリティエージェントは、アクセシビリティのベストプラクティスに従って Copilot がこれらの機能を実装できるようガイドしました。アクセシビリティに関する懸念や改善点がある場合は、前の PR と同様に、コメントで **@copilot** をタグ付けして更新を依頼できます。

## オプション演習 —委任を継続する

クラウドエージェントは、実際のバックログ項目を任せてほかの作業に集中できるときに、最も効果的です。この習慣を身につけるために、リポジトリに Issue を追加で作成し、Copilot に割り当てます。次の例があります。

- ゲーム詳細ページに支援者の関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## 概要

クラウドエージェントハーネスを完了しました。これらのレッスンでは、次の作業を行いました。

- このリポジトリに含まれるカスタム指示ファイルを**確認**し、クラウドエージェントの出力への影響を確認できました。
- **Issue を Copilot クラウドエージェントに割り当て**、環境をセットアップし、計画し、非同期で実行する様子を確認しました。
- アクセシビリティ用の**カスタムエージェントを作成および使用**し、ハイコントラストモードとライトモードのトグルを追加しました。
- **Copilot エージェントページを Mission control として使用**し、アクセシビリティセッションを進行中に監視および操作しました。
- **クラウドエージェントの pull request を確認して反復改善**し、`@copilot` をタグ付けして変更をリクエストしてワークフローを承認しました。

## 確認と次のステップ

クラウドエージェントハーネスを完了しました。引き続き確認する場合は、ほかのハーネスを使用すると、ここで実践した内容を補完できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェントモードと MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — ターミナルから Plan モードと Autopilot モード、エージェントスキル、カスタム QA エージェント、Playwright MCP、Agent Merge、およびクラウドエージェントへ戻るオプションの `/delegate` ブリッジを使用して、レビュー済みの 2つの変更を実装します。

ご自身のリポジトリで、次のようなフォローアップのアイデアを試せます。

- リファクタリングまたはテストカバレッジに関する Issue をクラウドエージェントに割り当てる。
- 別の分野向けのカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` をセットアップする。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [Copilot に Issue を割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot クラウドエージェントのファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
