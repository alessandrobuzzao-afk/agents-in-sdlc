---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI と作業する際にはコンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot が把握すべき背景情報がある場合は、そのコンテキストを確実に参照できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。その中でも重要なのは、コード生成に関するガイダンスを提供する[指示ファイル][instruction-files]です。

この演習では、次の作業を行います。

- リポジトリのカスタム指示とパススコープ付き指示ファイルを介して、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot にどのように届くかを確認します。
- 現在の指示が適用された状態で、フィルタリング用の最初のデータスライス（パブリッシャーヘルパー）を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい標準を追加します。
- 同じプロンプトを再実行し、生成されたコードが新しい標準を採用する様子を確認します。
- 更新した指示とフィルタリング用スライスを `main` にコミットし、次の演習で Copilot が更新されたガイダンスを使用できるようにします。

> [!CAUTION]
> 生成されたコードは、設定した標準の一部と異なる場合があります。Copilot の動作は非決定的です。この演習の目的は、指示を更新した後に動作の傾向が変化することを確認することであり、出力を文字単位で一致させることではありません。

## 指示ファイル

### シナリオ

優れた開発チームである Tailspin Toys には、開発手法に関するガイドラインと要件があります。その内容には次のものが含まれます。

- データレイヤーには常に単体テストが必要です。
- UI はダークモードでモダンな外観にします。
- コードには TSDoc ドキュメントコメントの形式でドキュメントを追加します。
- 各ファイルの先頭に、ファイルの目的を説明するコメントブロックを追加します。

指示ファイルを使用することで、強調された作業手法に沿ってタスクを実行できるよう、Copilot に適切な情報を提供します。

### カスタム指示

カスタム指示を使用すると、Copilot にコンテキストと設定を提供し、コーディングスタイルや要件をより正確に理解させられます。この強力な機能は、より関連性の高い提案やコードスニペットを得るためのガイダンスに役立ちます。推奨するコーディング規約、ライブラリ、使用を希望するコメントの種類などを指定できます。リポジトリ全体または特定のファイル形式に対して、タスクレベルのコンテキストに応じた指示を作成できます。

指示ファイルには 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送信される単一の指示ファイルです。このファイルには、使用技術、構築中のプロジェクトの概要、ベストプラクティス、ほかのグローバルなガイダンスなど、プロジェクトレベルの情報を記述します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイル形式向けに作成できます。TypeScript や Astro などの特定の言語、UI コンポーネントや新しい単体テスト一式の作成などのタスクに関するガイドラインを提供できます。

> [!NOTE]
> IDE で作業する場合、指示ファイルが使用されるのは Copilot Chat でのコード生成だけで、コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot クラウドエージェントは、コードを生成する際にリポジトリレベルの指示と、`applyTo` フロントマターを含む `*.instructions.md` ファイルの両方を使用します。
>
> 最後に、Copilot は AGENTS.md や CLAUDE.md ファイルなど、ほかの標準を使用する指示ファイルも[サポートしています][custom-instructions-support]。

### 指示ファイル管理のベストプラクティス

指示ファイルの作成について詳しく説明するには、このワークショップの範囲を超えます。ただし、サンプルプロジェクトにある例は代表的なアプローチを示しています。全体としては、次のようにします。

- `copilot-instructions.md` の指示は、構築中のプロジェクトの説明、プロジェクト構造、グローバルなコーディング標準など、プロジェクトレベルのガイダンスに絞ります。
- `*.instructions.md` ファイルは、ファイル形式（単体テスト、Astro コンポーネント、データレイヤー）や特定のタスク向けの具体的な指示を提供します。
- 自然言語を使用します。ガイダンスを明確にします。コードがどのような状態にするべきか、避けるべきかの例を示します。

指示ファイルの作成方法は 1 つだけではありません。AI の利用方法も同様です。試行錯誤を通して、プロジェクトに最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使用するすべてのプロジェクトには、充実した指示ファイルの集合が必要です。このプロジェクトの指示ファイルを確認すると、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク用のファイルがあることが分かります。
>
> Copilot は指示ファイルの生成も支援できます。各操作画面では方法が異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`）。画面に応じたレッスンで関連する方法を説明します。
>
> テンプレートや開始点を探していますか。[awesome-copilot][awesome-copilot] で、指示ファイル、カスタムエージェント、ほかのリソースを確認できます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認しましょう。コアの `copilot-instructions.md` が 1 件あり、さまざまなタスク向けの `*.instructions.md` ファイルの集合があります。エディターまたは GitHub Web UI でこれらを開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にあるネストされた **GitHub Actions Workflows** ガイダンスも確認します。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があることを確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドを確認します。リポジトリルートからの相対パターンで、指示を適用するファイルを指定します。この例では、TypeScript テストファイル（たとえば `**/*.test.ts` に一致するファイル）が対象になります。
5. プロジェクトで単体テストを作成するための指示を確認します。
6. 最後に、`.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。ほかの指示ファイル（`unit-tests.instructions.md`）やプロジェクト内の既存ファイルへのリンクがあることを確認します。これにより、大規模な指示セットを小さく再利用可能なファイルに分割し、Copilot がコード生成時に従う例を指定できます（ここでのパスはリポジトリルートではなく、指示ファイルからの相対パスです）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントに関するルールはまだありません。次のステップで、TSDoc ドキュメントコメントとファイルコメントヘッダーのルールを追加します。

## 変更用ブランチを作成する

ベストプラクティスに従って、変更用ブランチを作成しましょう。

1. 前の演習で作成した codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する*前*に Copilot Chat を使用する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を開始します。モードとモデルの選択は演習ごとに異なるため、各ステップの指示に従ってください。

カスタム指示の影響を確認するには、まず現在の指示ファイルを適用した状態でプロンプトを送信します。その後指示を更新し、同じプロンプトを再送して違いを確認します。

1. 前の演習で開いていたエディタータブを閉じ、Copilot が必要なコンテキストだけを参照できるようにします。
2. ヘルパーを配置する場所を Copilot に伝えるため、`src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェント選択メニューから **Agent** を選択し、Copilot にファイル変更を適用させます。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調査し、ヘルパーファイルとテストにまたがるコード更新を適用します。
6. 提案されたヘルパーは、最初の引数として `db` クライアントを受け取り、パブリッシャーの型付き配列を返す型付き関数です。これは `.github/instructions/drizzle.instructions.md`（`src/lib/*.ts` に適用）にあるデータレイヤーの規約によるものです。
7. 提案されたコードには、TSDoc ドキュメントコメントとファイルレベルのコメントヘッダーがないことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する場合があります。その場合でも問題ありません。指示を更新した後の改善の一貫性を確認することが、この演習の目的です。

## 新しいリポジトリ標準を追加する

前述のとおり、`.github/copilot-instructions.md` はプロジェクトレベルの情報を Copilot に提供するためのファイルです。リポジトリのコーディング標準を文書化し、コード提案を改善しましょう。

1. `.github/copilot-instructions.md` を再度開きます。
2. **Code formatting requirements** セクションを確認します。通常は 27 行付近にあります。このセクションにはプロジェクトのコーディング標準が記載されていますが、インラインドキュメントに関するルールがないため、生成されたヘルパーにはドキュメントコメントがありません。
3. Copilot にファイルコメントヘッダーと TSDoc ドキュメントコメントを追加させるため、既存の標準の直後に次の Markdown 行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前の演習で確認したとおり、リポジトリレベルの指示ファイル（`.github/copilot-instructions.md`）はグローバルなガイダンスに使用でき、`*.instructions.md` ファイルは特定の言語、ファイル形式、タスクに使用できます。リポジトリレベルのファイルは、今追加したドキュメントコメントルールなどのプロジェクト全体の標準を記載する適切な場所です。

## プロンプトを再実行して変更を確認する

1. Copilot Chat に戻り、**New Chat** を選択してチャット履歴をクリアします。
2. `src/lib/publishers.ts` を再度開き、Copilot が正しいファイルに注目できるようにします。
3. 前回と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を Copilot に適用させ、次の演習で構築する実際のコードに反映させることができました。

## 指示とプッシュ対象ブランチをコミットする

指示ファイルはほかの資産と同様に、リポジトリ内のほかの項目と同じソース管理方法で管理できます。ブランチをコミットしてプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルウィンドウを開きます。
2. ターミナルで、指示の更新とヘルパーの変更が存在することを確認するため、次を実行します。

   ```bash
   git status
   ```

3. ターミナルで、指示の更新と、ヘルパー基盤用に Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルでブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたので、バックログにあるドキュメント標準の issue に関連付けてプルリクエストを作成します。手動で行うこともできますが、Copilot に代行させることもできます。プロジェクトには GitHub ツールが接続されています。

> [!NOTE]
> これは、Copilot が GitHub 上で操作を行う最初の演習です。プロジェクトテンプレートには GitHub のツールが構成済みなのでセットアップは不要です。Copilot がツールを初めて使用するとき、VS Code に GitHub へのサインインを求めるプロンプトが表示される場合があります。表示される手順に従って許可してください。この接続の仕組み（Model Context Protocol）は後の演習で確認します。

1. codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューから **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関連する issue を Copilot に検索させ、現在のブランチから PR を作成します。指示の更新と、今後のフィルタリング作業に向けて追加したパブリッシャーヘルパーの基盤の両方について記述するよう依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue の検索と PR の作成を開始します。
5. Copilot に GitHub 操作の実行を許可するよう求められたら、コマンドを確認し、必要に応じて **Allow** を選択します。
6. PR が作成されたら、Copilot に PR のマージと現在のブランチを `main` に戻すことを依頼します。次のプロンプトを使用します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub 操作とシェルコマンドの実行を許可するよう求められたら、コマンドを確認し、必要に応じて **Allow** を選択します。

GitHub Copilot の支援を受けて、プルリクエストを作成し、マージしました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する仕組みを確認し、VS Code の Copilot Chat を使って次の作業を行いました。

- 現在の指示が適用された状態でコード生成プロンプトを送信し、Copilot の生成結果を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードが新しい標準を採用する様子を確認しました。
- 指示の更新とヘルパー基盤を `main` にコミットし、次の演習で利用できるようにしました。

次は、これらの指示を [Agent Mode][next-lesson] で活用し、Copilot にコードベース全体で新機能を追加させます。

## リソース

- [GitHub Copilot カスタマイズ用の指示ファイル][instruction-files]
- [カスタム指示の作成に関するベストプラクティス][instructions-best-practices]
- [Copilot 向けカスタム指示を改善する 5つのヒント][copilot-instructions-five-tips]
- [個人用カスタム指示][personal-instructions]
- [Awesome Copilot - 指示ファイルやほかのリソースのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
