---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

[カスタムエージェント][custom-agents-concept]を使用すると、GitHub Copilot に特定のタスクや開発ワークフローの分野に合わせた専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルとしてエージェントを定義すると、特定の作業をより効果的に実行するための手順、ベストプラクティス、コーディングパターン、ドメイン固有の知識を Copilot に提供できます。チームは専門知識を再利用可能なエージェントとして共有できます。たとえば、[WCAG][wcag] 準拠を強制するアクセシビリティエージェント、安全なコーディングプラクティスに従うセキュリティエージェント、一貫したテストパターンを維持するテストエージェントなどがあります。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルとして定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターを配置し、その後にエージェントの動作、専門知識、指示を定義する Markdown プロンプトを記述します。

### カスタムエージェントとエージェントスキルの比較

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、一部に論理的な重複があります。どちらも主に Markdown ファイルで定義され、AI に操作方法を指示します。両者を明確に分けるなら、**カスタムエージェント**は作業者であり、**スキル**はツールです。

カスタムエージェントには独自のコンテキストウィンドウがあり、作業中にスキルやほかのエージェントをオーケストレーションできます。このラボでは、アクセシビリティカスタムエージェントがアクセシビリティガイドラインに照らしてサイトを確認して更新します。この作業の一環として、pull request ワークフロースキルやテストの実行と管理を行うスキルを呼び出せます。

> [!NOTE]
> カスタムエージェントを作成する唯一の正解はありません。AI を使用する場合と同様に、テストと反復を通じて、自分の環境とシナリオに最適な方法を見つけてください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタムエージェントでは、次のことを確認します。

- カスタムエージェントの定義方法
- カスタムエージェントにタスクを割り当てる方法

## シナリオ

Tailspin Toys は、クラウドファンディングプラットフォームを視覚能力や好みにかかわらず、すべてのユーザーが利用できるようにすることを目指しています。最近のユーザーフィードバックでは、テキストと背景色のコントラストが不足しているため、現在のダークテーマが読みにくいと指摘されています。このアクセシビリティ上の懸念に対応するため、デザインチームはユーザーがオンとオフを切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要な要素であるため、できるだけ迅速に実装できるようにします。カスタムエージェントを使用して機能を生成します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに作成されています。Copilot をどのようにガイドするか理解するため、その内容を確認します。

Codespace に戻り、アクセシビリティカスタムエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> `name` と `description` を含むフロントマターは、カスタムエージェントに必須です。

3. 続いて、次を示すセクションを確認します。
   - アクセシブルな Web サイト向けコードを生成する際の主要な責務
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある落とし穴と誤りの一覧

## Issue を作成して割り当てる

Mission control は、環境内のすべてのエージェントを操作するための中心的な場所です。Copilot クラウドエージェントにタスクを割り当て、タスクを監視し、作業を変更したり追加ガイダンスを提供したりできます。まず、ハイコントラストモードを作成するタスクを割り当てます。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue ダイアログを開きます。
4. **Blank issue** を選択して、新しい Issue を作成します。
5. **Title** に `Add high contrast mode to website` を設定します。
6. **Description** に次を設定します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てダイアログを開きます。
9. カスタムエージェントの一覧から **Accessibility agent** を選択します。

    ![カスタムエージェントとアクセシビリティエージェントが強調表示されたクラウドエージェントの割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドでタスクを開始します。

## 概要と次のステップ

このレッスンでは、特定のタスクや分野に合わせた専門的な AI アシスタントである GitHub Copilot の[カスタムエージェント][custom-agents]について確認しました。カスタムエージェントを使用すると、チームの専門知識と標準を再利用可能なエージェントとして共有し、Copilot が特定の種類の作業をより効果的に実行できるようにできます。

次の概念を確認しました。

- カスタムエージェントの定義方法
- カスタムエージェントにタスクを割り当てる方法

ハイコントラストモードの実装作業中に、Mission control から[エージェントセッションを監視および操作][next-lesson]する方法を確認します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [Organization でカスタムエージェントを使用する準備][org-custom-agents]
- [Enterprise でカスタムエージェントを使用する準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot クラウドエージェント][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
