---
title: "演習 3 - Agent モード: Plan モードと Autopilot モード"
description: "Plan モードで方針を合意し、Autopilot モードで issue に基づいてフィルタリングを構築し、Interactive モードで結果を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまで、小規模な機能をプロジェクトに追加しました。しかし、規模の大きな変更には、より堅牢なプロセスが必要です。幸い、GitHub Copilot CLI は組織の既存ワークフローに沿って動作するため、適切なものを適切な方法で構築できます。ここから複数の演習で、issue を使って新機能を作成し、コードの妥当性と機能の動作を確認し、最終的に pull request をマージするという、一般的な agent 主導の開発プロセスに従います。

> [!NOTE]
> 機能ワークフローを続けるため、同じ会話とブランチを使用します。通常は、ファイルの種類ごとに異なるブランチや PR を使いますが、ここでは中核となる概念に集中できるよう、手順を簡略化します。

この演習では、次のことを行います。

- GitHub issue から新しい Copilot conversation を開始する。
- Plan モードで要件を定義する。
- Autopilot モードで新機能を実装する。
- コードを確認する。
- 転送されたブラウザーで機能を手動検証する。

機能の作成を続けながら、リポジトリ instructions を更新し、既存の `quality-checks` skill をカスタマイズし、MCP 検証を追加し、QA agent を作成し、feature PR を作成します。

## シナリオ

Tailspin Toys のカタログは増え続けているため、利用者はカテゴリやパブリッシャーでゲームを絞り込める必要があります。バックログ issue に機能要件は記載されていますが、カテゴリの組み合わせ方などの詳細はコーディング前に合意が必要です。Plan モードで判断を確定し、Autopilot モードで範囲を限定した実装を承認します。

## 背景

AI coding agent を開発フローに導入しても、基本的な要素は変わりません。むしろ、従来どおり重要です。ほとんどの開発者は、次のような流れに従います。

1. 実施内容を記述した issue から開始する。
2. 構築するもののプランを作成する。
3. コードを作成して確認する。
4. テストを実行してコードを検証する。
5. 新機能を手動で検証する。
6. pull request (PR) を作成する。
7. コードがレビューされ、continuous integration プロセスが成功したらマージする。

> [!NOTE]
> チームや組織によって具体的な手順は異なりますが、ほとんどは上記の基本的な流れを基にしています。

この標準的な方法に沿うことで、AI が生成したコードは要件を満たし、手作業で作成したコードと同じ検証プロセスを通過します。

## conversation モード

**conversation mode** は、agent に与える自律性を制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えます。

- **Interactive**: 開発者と agent が協力します。agent は変更案を提示し、処理を進める前に開発者の入力を待ちます。
- **Plan**: agent は最初にプランを作成し、プロジェクト ファイルの編集は行えません。
- **Autopilot**: agent はコードの作成、テストの実行、タスク完了までの反復を自律的に行います。

まず Plan モードで開始し、プランを確認してから、Autopilot モードで実装します。

## issue から開始する

フィルタリングの作業を開始する前に、Codespace に戻り、リポジトリとターミナルの準備を確認します。

1. Codespace に戻ります。停止している場合は、続行前に再起動します。
2. star-rating PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）を押します。
4. `main` を更新し、フィルタリング作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーを使い、**Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトは `#7`（または同様の番号）で始まります。`#` を使うと、GitHub の issue または pull request (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

プランを作成すると、Copilot に作業を任せる前に、機能の実装方法やタスクの実施方法を定義できます。複雑な作業では時間をかけて計画することを推奨します。プラン モードに切り替え、Copilot にプランを作成してもらいます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下に表示されるモードインジケーターが **Plan** になったことを確認します。
2. 前の手順で追加した issue reference の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプランの作成を開始します。まずプロジェクトを調査し、最適な方法を判断します。

3. フィルタリング機能の動作について Copilot が質問する場合があります。希望する内容に基づいて回答します。正解はありません。
4. プランが完成したら、<kbd>Control</kbd>+<kbd>E</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）を押してプランを展開します。
5. 上下にスクロールしてプランを確認します。
6. 判断した方針と一致しない部分があれば、Copilot にプランの修正を依頼します。

## Autopilot を承認する

プランの作成と確認が完了したので、実装を開始します。Copilot に作業を任せるため Autopilot を使用します。

Autopilot を使うと、タスクが完了したと判断するまで問題の反復的な解決を自動で行います。

1. **Accept plan and build on autopilot (recommended)** または、インストールされているバージョンで同様の名前のオプションを選択します。
2. プロンプトの下に表示されるモードインジケーターが **Autopilot** になったことを確認します。
3. Copilot が確定したプランに沿って反復し、コードを生成し、テストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が開始されるため、プランを先に確認してください。Copilot が依存関係の不足やポート競合を報告した場合は、checks を完了とみなす前にセットアップの問題を解消してください。

## 実装を確認して検証する

コードが生成されたら、マージ前に他のコードと同様に確認する必要があります。コードを確認し、サイトを起動して動作を確認します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モードインジケーターに **Plan** または **Autopilot** が表示されなくなったことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. 結果を issue と計画時に決定した方針と比較します。
4. コードの確認が完了したら、<kbd>Esc</kbd> を押して diff view を終了します。
5. プロジェクト checks の出力を確認し、失敗があれば Copilot に解消を依頼します。

## 新機能を確認する

コードは問題なさそうですが、実際に動作するでしょうか。前回と同様に app を起動し、Codespaces の転送ポートからサイトを開きます。

1. app を起動するよう Copilot に依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces がポート `4321` が利用可能になったと通知したら、**Open in Browser** を選択します。
3. カテゴリ フィルター、パブリッシャー フィルター、およびプランで合意した組み合わせを試します。
4. リセット動作と0件時の動作が issue と判断した方針に一致することを確認します。
5. Codespace に戻り、Copilot に起動した development server の停止を依頼します。

## まとめと次のステップ

異なる conversation モードを使って、機能の構築と確認を行いました。この演習では、次のことを行いました。

- GitHub issue から新しい Copilot conversation を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードを確認しました。
- 転送されたブラウザーで機能を手動検証しました。

次は、コードが文書化されたプラクティスに従うようにするため、[custom instructions を使用][next-lesson]して作業を深掘りします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
