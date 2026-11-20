---
title: "演習 7 - QA agent を作成して使用する"
description: "issue 要件、quality-checks skill、Playwright MCP を組み合わせた QA custom agent を作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` skill を使って自動 checks を実行し、Playwright MCP を使ってブラウザー上のフィルタリングを確認しました。次は、明確に定義された QA プロセスを持つ custom agent にこれらの機能を統合します。

この演習では、次のことを行います。

- custom agent が instructions、skills、MCP tools と連携する仕組みを確認する。
- 再利用可能な quality assurance プロファイルを作成して確認する。
- QA agent を選択し、フィルタリング issue に照らして結果を確認する。

## シナリオ

Tailspin Toysでは、pull request (PR) を作成する前に要件、コード品質、自動 checks、テスト カバレッジ、ブラウザー動作を一貫して確認したいと考えています。custom agent を使うと、この QA プロセスを調整し、再利用可能なレポートを提供できます。

## custom agentとは

custom agent は、Markdown プロファイルで定義する Copilot の専門バージョンです。プロファイルには、agent の目的、instructions、利用可能な tools を記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA role を定義し、Copilot CLI で選択します。

これまで使用したカスタマイズには、それぞれ異なる役割があります。リポジトリ instructions はチームの標準を記述し、`quality-checks` skill は反復可能な checks をまとめ、Playwright MCP はブラウザー tools を提供します。QA プロファイルは、これらの機能を使って要件を評価し、結果をレポートする方法を Copilot に指示します。カスタマイズは機能を置き換えるものではなく、別の会話を必要とするものでもありません。

## QA プロファイルを作成する

feature PR を作成する前に、再利用可能な QA プロファイルを作成します。プロファイルは、QA が実行する checks と遵守する境界を定義します。

1. Codespace に戻り、フィルタリング conversation が Interactive モードになっていることを確認します。
2. default agent に新しい custom agent の作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しい agent を使用する前に、プロファイルを確認し、Copilot が意図した QA ワークフローと境界を反映していることを確認します。これにより、機能の検証だけを目的としている場合に、不完全または範囲の広すぎる agent が変更を加えることを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を確認します。`description` は必須です。`name` は任意ですが、指定すると agent の表示名が明確になります。
3. プロファイル instructions を読み、QA が要件から開始し、リポジトリ instructions に従い、`quality-checks` skill を実行し、Playwright MCP を使用することを確認します。
4. QA が裏付けとなる証拠をレポートし、implementation code を変更する前に確認し、変更をコミットしたり pull request を作成したりしないことを確認します。
5. 生成されたプロファイルに責任または境界が欠けている場合は、続行前に default agent に修正を依頼します。

## issue に照らして QA を実行する

Copilot CLI は conversation の開始時に project agents を読み込みます。プロファイル作成後、同じフィルタリング conversation を再開し、QA を選択して issue と計画時の判断をコンテキストとして使用します。

1. 次のプロンプトで agent を有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> agent を作成したばかりで、利用可能な agent の一覧に表示されない場合があります。ただし、agent は存在するため、上記のコマンドで有効化できます。

2. 次のプロンプトを使い、QA agent に機能の確認を依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA agent の作業を確認します。
4. 作業完了後に提供されたレポートを確認します。

## まとめと次のステップ

再利用可能な専門 role をワークフローに追加し、その作業を確認できました。この演習では、次のことを行いました。

- custom agent が instructions、skills、MCP tools と連携する仕組みを確認しました。
- 再利用可能な quality assurance プロファイルを作成して確認しました。
- QA agent を選択し、フィルタリング issue に照らして結果を確認しました。

implementation、skill の更新、QA プロファイル、テスト、検証レポートを確認できる状態になりました。次は、[feature PR にまとめ、Agent Merge を使用][next-lesson]します。

## リソース

- [Copilot CLI 用の custom agents を作成する][create-agents]
- [Custom agent の設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
