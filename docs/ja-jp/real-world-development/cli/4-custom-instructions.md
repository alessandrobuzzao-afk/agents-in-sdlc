---
title: "演習 4 - custom instructions を使って Copilot をガイドする"
description: "リポジトリ instructions を確認し、ドキュメント標準を追加し、フィルタリング コードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使用する際は、コンテキストが重要です。タスクを特定の方法で実施する必要がある場合は、そのガイダンスを Copilot が利用できるようにする必要があります。[Instruction files][instruction-files] は、必要なコードだけでなく、その構造方法も記述します。フィルタリングを構築したため、Copilot が使用した instructions を確認し、ドキュメント標準を追加し、コードに適用します。

この演習では、次のことを行います。

- リポジトリ instructions とパス スコープ instructions が agent にどのように渡されるかを確認する。
- instructions ファイルを更新してコーディング標準を確実に守る。
- instruction files がコードに与える影響を確認する。

## シナリオ

優良な開発チームである Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。具体的には、次のとおりです。

- コメントはコードを言い換えるのではなく、意図や非自明な判断を説明する。
- `db/` と `src/lib/` の exported functions は、目的、パラメーター、戻り値を TSDoc/JSDoc で文書化し、該当する場合は injectable `db` 引数も含める。
- 再利用可能な Astro コンポーネントは `Props` contract を文書化し、関連コードが変更された場合はコメントも最新の状態を保つ。
- 既存のフォーマットおよび lint ガイダンスを維持する。

instruction files を使うことで、強調されたプラクティスに沿ってタスクを実行できる適切な情報を Copilot に提供します。

## Instruction files

Custom instructions を使うと、コンテキストや設定を Copilot に提供し、コーディング スタイルや要件を理解しやすくできます。この機能は、より適切な提案やコード スニペットを得るための強力な手段です。リポジトリ全体または特定のファイル種別に対して、好みのコーディング規約やライブラリ、コメントの種類を指定できます。

instruction files は2種類あります。

- `.github/copilot-instructions.md`: リポジトリに対する **すべて** のリクエストに Copilotへ送られる単一の instruction file。リポジトリ全体に関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md`: 特定の言語、ファイル形式、タスク向けのガイドラインを提供します。

> [!NOTE]
> Instruction format とサポートは環境によって異なる場合があります。特定の形式に依存する前に、[custom instructions support reference][custom-instructions-support] を確認してください。

## このプロジェクトの custom instructions ファイルを確認する

開始しやすくするため、スターター プロジェクトには instruction files があらかじめ含まれています。変更による影響を確認するため、変更前に既存の内容を確認します。

1. Codespace に戻ります。
2. Codespaces エディター（ターミナルではなく）で `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを記述した簡潔なファイルを確認します。これらの instructions は、このリポジトリで Copilot と行うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle data layer、tests などの instructions があることを確認します。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` field に注目してください。この glob は instructions を適用するファイルを決定します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の instruction files や既存プロジェクト ファイルへのリンクを確認します。これにより、大規模な instruction sets を小さな再利用可能なファイルに分割し、Copilot が従う例を参照させることができます。

## チームガイダンスに合わせて instruction files を更新する

既存のファイルは有用ですが、まだ不足があります。新しく生成する TypeScript に TSDoc comments を確実に追加するため、`copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクションを探します。約35行目です。
2. 次の内容を最後の箇条書きとして追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使用する

Copilot CLI は conversation の開始時にリポジトリ instructions を読み込みます。更新した instructions を保存したら、フィルタリング conversation を終了して再開し、機能コンテキストを失わずに新しいガイダンスを読み込ませます。

1. フィルタリング conversation が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動保存します。
2. リポジトリのルートから `game-filters-cli` ブランチで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロン​​プトが表示されたらフィルタリング conversation を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新されたガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。生成された新しい TSDoc comments がコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が instruction files からコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリ instructions とパス スコープ instructions が agent にどのように渡されるかを確認しました。
- instructions ファイルを更新してコーディング標準を確実に守りました。
- instruction files がコードに与える影響を確認しました。

次は、[再利用可能な quality-checks skill をカスタマイズして実行][next-lesson]し、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI に custom instructions を追加する][instruction-files]
- [Custom instructions のサポート][custom-instructions-support]
- [Custom instructions 作成のベストプラクティス][instructions-best-practices]
- [Awesome Copilot - instruction files その他のリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
