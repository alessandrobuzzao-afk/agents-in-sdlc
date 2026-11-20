---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]では、開発ワークフローの特定のタスクや分野に合わせた専用 AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルでエージェントを定義すると、特定の種類の作業をより効果的に実行できるよう、注目すべき指示、ベストプラクティス、コーディングパターン、ドメイン固有の知識を Copilot に提供できます。チームの専門知識を再利用可能なエージェントとして体系化できます。たとえば、[WCAG][wcag]への準拠を強制するアクセシビリティエージェント、セキュアコーディング手法に従うセキュリティエージェント、一貫したテストパターンを維持するテストエージェントです。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルの `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターを設定し、その後にエージェントの動作、専門分野、指示を定義する Markdown プロンプトを記述します。

### カスタムエージェントとエージェントスキルの比較

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、機能面で部分的な重複があります。どちらも主に Markdown ファイルで定義され、AI に操作方法を指示します。両者を分ける簡潔な考え方は、**カスタムエージェント**が作業者で、**スキル**がツールだというものです。

カスタムエージェントには固有のコンテキストウィンドウがあり、作業を進める際にスキルやほかのエージェントをオーケストレーションできます。このラボでは、アクセシビリティ用カスタムエージェントがアクセシビリティガイドラインに照らしてサイトをレビューおよび更新します。その作業の一環として、プルリクエストワークフローのスキルやテストの実行と管理を行うスキルを呼び出せます。

> [!NOTE]
> カスタムエージェントを作成する「正しい」方法は 1 つだけではありません。ほかの AI 活用と同様に、環境やシナリオに合う形を見つけるため、テストと反復を行ってください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚的な能力や設定の違いにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるよう、アクセシビリティへの取り組みを継続しています。最近のユーザーフィードバックでは、テキストと背景のコントラストが不十分なため、現在のダークテーマを読みづらいと感じるユーザーがいることが分かりました。このアクセシビリティ上の懸念に対応するため、デザインチームはユーザーがオン・オフを切り替えられるハイコントラストモードの実装を要望しています。

アクセシビリティは重要なため、できるだけ迅速に実装する必要があります。この機能の生成にはカスタムエージェントを利用します。

この演習では、次の作業を行います。

- 既存のアクセシビリティ用カスタムエージェントをレビューします。
- Copilot Chat でアクセシビリティエージェントを使用し、ハイコントラストモードを実装します。

## アクセシビリティ用カスタムエージェントをレビューする

アクセシビリティ用のカスタムエージェントはすでに作成されています。Copilot をどのようにガイドするかを理解するため、内容を確認します。

codespace に戻り、ターミナルを開いて、アクセシビリティ作業用に `main` から新しいブランチを作成します（演習 3 のフィルタリング用 PR は別に維持します）。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続いて、次の内容を示すセクションを確認します。
   - アクセシブルな Web サイトを生成する際の主要な責任。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある落とし穴と問題の一覧。

## Copilot Chat でカスタムエージェントを使用する

VS Code は、`.github/agents` に定義されたすべてのカスタムエージェントを Copilot Chat ビューのエージェント選択メニューに表示します。カスタムエージェントを選択すると、そのエージェントの指示とツールを対象にチャットセッションを絞り込めます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を開始します。モードとモデルの選択は演習ごとに異なるため、各ステップの指示に従ってください。

1. エージェント選択メニューで、まだ **Agent** が選択されていない場合は選択します。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. チャットビュー下部のエージェント選択メニューを選択します（表示されるエージェントは現在アクティブなエージェントで、既定では **default** です）。
3. 利用可能なエージェント一覧から **Accessibility agent** を選択します。
4. アクセシビリティエージェントに次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat が作業を開始し、コードベースを調査して変更を提案し、プロジェクトファイルを編集します。各編集はチャット内にインライン表示され、ファイルパスとレビュー可能な差分が示されます。
6. 編集が反映されると、エージェントが変更したファイル一覧を確認できるように **Files changed** インジケーターが更新されます。

> [!NOTE]
> この処理には数分かかる場合があります。Copilot はリポジトリに実際の変更を加え、作業中に Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集します。

この進行中の作業は、次の演習でレビューして調整します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた専用 AI アシスタントである GitHub Copilot の[カスタムエージェント][custom-agents]について説明しました。カスタムエージェントを使うと、チームの専門知識と標準を再利用可能なエージェントとして体系化し、Copilot に特定の種類の作業を効果的に実行させることができます。

次の概念を確認しました。

- カスタムエージェントの定義方法。
- Copilot Chat のエージェントモードでカスタムエージェントを使用する方法。

次は、[エージェントの作業を監視して方向付け][next-lesson]します。変更が行われる様子を確認し、同じセッションでライトモードの切り替えを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
