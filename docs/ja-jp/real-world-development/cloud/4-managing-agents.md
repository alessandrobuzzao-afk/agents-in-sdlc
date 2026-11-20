---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前回の演習では、Copilot クラウドエージェントに、ユーザーエクスペリエンスの改善と機能追加を目的とした 3つのタスクを依頼しました。クラウドエージェントは非同期かつ自律的に動作しますが、これらのタスクを監視できることも重要です。

GitHub.com の[エージェントページ][agents-page]など、クラウドエージェントに割り当てたタスクを管理するためのツールが多数あります。この Mission control から、open 状態の pull request (PR) があるすべてのエージェントタスクを確認できます。実行された操作を確認し、進行中のセッションを操作してガイダンスを調整できます。

このレッスンでは、次の作業を行います。

- エージェントページを確認してクラウドエージェントタスクを監視します。
- 進行中のセッションを操作して、追加機能をリクエストします。

## シナリオ

ハイコントラストモードの作成をエージェントに依頼した後、チームではライトモードも追加するとよいことに気づきました。すでにサイトのスタイル更新とトグル機能の追加が進行しているため、この機能も含めるのは自然な判断です。ハイコントラストモードに加えてライトモードも追加できるよう、エージェントの作業を操作します。

## Copilot クラウドエージェントのタスクを確認する

Copilot クラウドエージェントに割り当てられているすべてのタスクの現在の状態を確認します。

1. [Copilot エージェントページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインのタスク一覧を確認します。これまでに Copilot に割り当てたタスクが表示されます。たとえば、次があります。
    - コードベースのドキュメントを更新する。
    - ゲームを変更する API を生成する。
    - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを 1つ選択します。Copilot が実行したタスクを確認します。次のような操作が含まれる場合があります。
    - リポジトリからコードをチェックアウトする。
    - Copilot の作業環境を作成する。
    - MCP サーバーをセットアップする。
    - 割り当てられたタスクを完了するためのさまざまな手順を実行する。

> [!NOTE]
> 表示される正確な手順は、Copilot の作業状態や使用したアプローチによって異なります。

4. 右側に表示される pull request (PR) ペインにも注目します。監視を強化するため、PR と変更されたファイルを確認できます。

## クラウドエージェントを操作する

アクティブなタスクを確認できたので、ハイコントラストモードの作業中にライトモードのトグルも追加するよう Copilot に依頼します。

1. ハイコントラストモードの追加に関するセッションを選択します。Copilot が使用する名前と作業の現在の状態によって、正確なタイトルは異なります。

    ![Mission control のアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を開始したことが表示されるまで、数分間セッションを確認します。次のようなメッセージが表示され始めれば、作業が開始されています。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを追加します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェントページで、作業中の Copilot セッションを誘導するダイアログが強調表示されたクラウドエージェントタスク画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを認識し、作業フローに組み込む様子を確認します。

## Copilot に作業を任せる

前回と同様に、Copilot が更新されたタスクを実行します。メッセージを送信した時点で Copilot が実行していた特定のステップが完了した後、新しいリクエストを作業フローに組み込みます。

この作業にも数分かかるため、これまで学び確認してきた内容を振り返る時間にします。

## 概要と次のステップ

このレッスンでは、GitHub Copilot クラウドエージェントのタスクを監視および誘導するための中央ハブである Copilot エージェントページについて確認しました。この Mission control を使用すると、実行中および完了済みのすべてのタスクを追跡し、実行中の作業を確認し、進行中のタスクをリダイレクトしてスコープを調整したり、追加ガイダンスを提供したりできます。

次の概念を確認しました。

- Mission control とエージェントページを確認してクラウドエージェントタスクを監視しました。
- 進行中のセッションをリダイレクトして、追加機能をリクエストしました。

Copilot がアクセシビリティ機能の実装を完了したら、作成された [pull request を反復的に改善][next-lesson]します。

## リソース

- [Copilot エージェントページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復的に改善 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
