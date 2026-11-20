---
title: "レッスン 5 - quality-checks スキルのカスタマイズと使用"
description: "既存の quality-checks スキルを確認し、報告形式をカスタマイズして、フィルター機能の検証に使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書く作業には、コードを書くこと以外の作業も含まれます。コードが動作することは手動で検証でき、指示ファイルを使って標準に従っていることも確認できました。しかし、テスト、lint、継続的インテグレーション (CI) のほかの要素はどうでしょうか。

このようなタスクには、**エージェントスキル**が最適です。スキルを使用すると、これらの処理を適切に実行する方法を Copilot が理解できます。

このレッスンでは、次の内容を学習します。

- 既存の `quality-checks` スキルを確認する。
- 結果の報告形式をカスタマイズする。
- スキルを再読み込みして実行する。

## シナリオ

Tailspin Toys は、単体テスト、lint、型チェックに `quality-checks` スキルを使用します。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルは、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースをまとめたもので、エージェントが必要に応じて読み込みます。基本的には、スキル名のフォルダーと、その中の Markdown ファイル `SKILL.md` で構成されます。Markdown のフロントマターには、スキルの名前と説明、スキルの概要、呼び出すタイミングのガイダンスが含まれます。フォルダーには、スキルの呼び出し時に使用するスクリプトやその他のリソースを収めたサブフォルダーを含めることもできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトの既存コマンドを使用するため、`SKILL.md` のみを含みます。

スキルは、チームで共有および再利用できるリポジトリアセットとしてプロジェクトの `.github/skills` フォルダーに配置できます。または、`~/.copilot/skills` のユーザースキルフォルダーに配置できます。

## スキルを確認する

Tailspin Toys チームが単体テスト、lint、型チェックの実行用に作成した `quality-checks` スキルを確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 冒頭の `name` と `description` を読みます。この説明は、Copilot がスキルを呼び出すタイミングを判断するために使用されます。
3. 指示を読み、テストと lint のプロセスを Copilot にどのように案内しているかを確認します。
4. スキルにまだ **Results output formatting** セクションがないことを確認します。

## 変更前にスキルを実行する

スキルは Copilot CLI から直接呼び出すか、自然言語で呼び出せます。スキルの 3 種類のチェックを実行するよう Copilot に依頼しましょう。

1. Interactive モードでフィルターの会話に戻ります。
2. 次のプロンプトを使用します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行内容、成功したかどうか、ツールが実際に報告した内容がわかる、よりよいレポートが必要です。そのレポートを作成するようスキルを更新しましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を反映して動作を確認します。Copilot CLI は、会話を再起動せずに編集したスキルを再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じプロンプトを使用します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして使用しました。このレッスンでは、次の作業を行いました。

- 単体テスト、lint、型チェックを実行する `quality-checks` スキルを確認した。
- 結果の報告形式をカスタマイズした。
- スキルを再読み込みして実行した。

この変更は、フィルター機能と一緒に機能の PR に含めます。次は、[Playwright MCP サーバーを通じて][next-lesson] Copilot がサイトを直接操作できるようにします。

## ほかのスキルの例

これらのコミュニティの例は参考資料であり、追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件文書: `prd`][prd-example]
- [図と同梱のエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
