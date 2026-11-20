---
title: "演習 10 - まとめと次のステップ"
description: "Copilot CLI ワークフロー、2つの pull request、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の継続的なワークフロー全体を進めました。具体的には、次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認し、フィルタリング issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認し、最初の pull request (PR) を手動でマージしました。
- フィルタリング issue から開始し、Plan モードで方針を定義し、Autopilot モードで構築し、Interactive モードで確認しました。
- custom instructions で agent をガイドし、既存の `quality-checks` skill をカスタマイズして単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- quality assurance (QA) custom agent を作成して選択し、要件、カバレッジ、skill の結果、ブラウザーの証拠を評価しました。
- フィルタリングの完全な変更を確認し、フィルタリング PRの Agent Merge を承認しました。
- コンテキスト、モデル、共有、任意の cloud delegation を扱うスラッシュ コマンドを確認しました。

## 作成したもの

ワークショップには、更新した `main` からそれぞれ別のブランチで作成する2つの PR マイルストーンがあります。

1. **星評価:** ゲームカードに既存の `starRating` と明示的な未評価状態を表示する。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、instructions を更新して機能に適用し、`quality-checks` レポートをカスタマイズし、QA プロファイルを作成し、関連テストを追加する。

フィルタリングの計画から PR 作成まで、同じ会話とブランチを使用しました。ワークショップを効率化するため、これらの作業を1つの PR にまとめました。

## 検証の種類

コードを複数の方法で確認しました。自動テスト、自身によるブラウザー確認、MCP を通じた Copilot のブラウザー確認です。`quality-checks` skill は単体テスト、lint、型チェックを実行し、新しい形式でレポートしました。QA は要件とテスト カバレッジの確認と併せて、これらの結果をまとめました。

追加するテストは真の不足を埋める必要があります。新しいテストを追加する必要がない QA 実行でも正しい場合があります。マージを承認する前にコードと証拠を確認し、変更後は影響を受ける証拠を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、その作業に影響します。このワークショップでは instructions を更新し、skill をカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。チームのニーズに応じてカスタマイズを会話間で再利用し、必要に応じて調整してください。instructions は標準を設定し、skills は反復可能なタスクを記述し、custom agents は専門 role を定義し、MCP サーバーは外部ツールを接続します。agent の要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて **モードとモデル** を選択します。構築前に方針を考えるときは **Plan**、焦点を絞った変更では常に作業状況を把握する **Interactive**、スコープが明確なタスクでは **Autopilot** を使用します。定型的な編集には高速なモデル、複雑な作業には高性能なモデルを選択します。

インフラと同じくらい、コンテキストが重要です。何を作成するか、なぜ作成するか、どのように作成するかを明確に記述すると、出力に大きく影響します。

## さらに確認する内容

中核となるワークフローは完了しました。CLI のその他の機能として、次のものがあります。

- `/review`: code review agent に変更の分析を依頼する。
- `/rubber-duck`: 問題を話し合い、別の視点を得る。
- `/fleet`: 独立したサブタスクを並行して調整する。
- `/worktree`: 別のタスクを分離する。
- `/delegate`: タスクを Copilot cloud agent に送る。

## 次のステップ

どのツールも使い続けることが上達の最善策です。本番コード、趣味のコード、何年も作りたいと思っている小さな app に活用してください。チームに学びを共有し、チームから学びましょう。いつものように、ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進んでください。他の環境と比較する場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLIについて][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [pull request を管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
