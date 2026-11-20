---
title: "演習 1 - カスタム指示 (クラウドエージェント)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot クラウドエージェント →][next-lesson]

生成 AI と連携する際は、コンテキストが重要になります。特定の方法でタスクを実行する必要がある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストへ確実にアクセスできるようにする必要があります。[指示ファイル][instruction-files]を使用してガイダンスを提供することで、Copilot は実現してほしいことだけでなく、実現方法も理解できます。

この演習では、次の作業を行います。

- リポジトリのカスタム指示とパススコープの指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot にどのように提供されるかを確認します。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の標準を追加します。

> [!NOTE]
> VS Code ハーネスや CLI ハーネスとは異なり、この演習では*実行前と実行後*のプロンプトを使用しません。Copilot クラウドエージェントは GitHub Issue 上で非同期に動作するため、その影響をリアルタイムで並べて確認することは難しいためです。クラウドエージェントが生成した pull request を確認すると、このハーネスの後半で指示ファイルの影響を確認できます。

## 指示ファイル

### シナリオ

優れた開発チームである Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。その一部は次のとおりです。

- データ層には常に単体テストが必要です。
- UI はダークモードでモダンな印象にします。
- コードには TSDoc ドキュメントコメントの形式でドキュメントを追加します。
- 各ファイルの先頭に、そのファイルの目的を説明するコメントブロックを追加します。

指示ファイルを使用することで、Copilot が示されたプラクティスに沿った作業を行うために必要な情報を確実に渡せます。

### カスタム指示

カスタム指示を使用すると、Copilot にコンテキストと設定を提供し、コーディングスタイルや要件をより正確に理解させることができます。この強力な機能を使用すると、より適切な提案やコードスニペットを得られるよう Copilot を誘導できます。好みのコーディング規約、ライブラリ、コードに含めたいコメントの種類を指定できます。リポジトリ全体または特定のファイル種類に対して、タスクレベルのコンテキストを提供する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストについて Copilot に送信される単一の指示ファイルです。プロジェクトレベルの情報、つまりチャットまたは CLI リクエストで共有されるほとんどのコンテキストに関連する情報を含めます。技術スタック、構築しているものの概要、ベストプラクティス、その他の全般的なガイダンスなどを記載できます。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイル種類向けに作成できます。TypeScript や Astro などの特定の言語、UI コンポーネントの作成、新しい単体テスト群の作成などに関するガイドラインを提供できます。

> [!NOTE]
> IDE を使用する場合、指示ファイルは Copilot Chat によるコード生成時にのみ使用され、コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot クラウドエージェントは、コード生成時にリポジトリレベルのファイルと `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使用します。
>
> 最後に、Copilot は [AGENTS.md や CLAUDE.md ファイルなど、ほかの標準を使用する指示ファイル][custom-instructions-support]もサポートしています。

### 指示ファイル管理のベストプラクティス

指示ファイルの作成に関する詳細な説明は、このワークショップの範囲外です。ただし、サンプルプロジェクトで示されている例は代表的なアプローチです。概要は次のとおりです。

- `copilot-instructions.md` の指示は、構築しているものの説明、プロジェクト構造、全般的なコーディング標準など、プロジェクトレベルのガイダンスに絞ります。
- `*.instructions.md` ファイルを使用して、ファイル種類（単体テスト、Astro コンポーネント、データ層）または特定のタスクに関する具体的な指示を提供します。
- 自然言語を使用します。ガイダンスは明確にします。コードがどのような状態であるべきか、どのような状態であるべきでないかの例を示します。

AI の使用方法と同じく、指示ファイルの作成方法にも 1つの正解はありません。実験を通じて、自分のプロジェクトで最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使用するすべてのプロジェクトに、充実した指示ファイル群を用意することをお勧めします。このプロジェクトに含まれるファイルを確認すると、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、多数のタスク向けファイルがあることがわかります。
>
> Copilot を利用して指示ファイルを生成することもできます。各インターフェイスでの操作方法は異なります（たとえば、VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init`）。対象のインターフェイスに関連する箇所でレッスン内に記載します。
>
> テンプレートや開始点を探していますか。[awesome-copilot][awesome-copilot] を確認してください。このリポジトリには、指示ファイル、カスタムエージェント、その他のリソースがまとめられています。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認します。コアとなる `copilot-instructions.md` 1 ファイルと、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI でこれらを開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要に加え、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** の入れ子のガイダンスにも注目してください。このガイダンスは Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示があることを確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドを確認します。このフィールドは、指示の適用対象となるファイルをリポジトリルートからの glob パターンで指定します。ここでは、TypeScript テストファイル（たとえば `**/*.test.ts` に一致するファイル）が対象になります。
5. このプロジェクトで単体テストを作成するための具体的な指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` や既存ファイルへの参照など、ほかの指示ファイルへのリンクを確認します。これにより、大規模な指示セットを、より小さな再利用可能なファイルに分割し、Copilot がコード生成時に参照する例を指定できます。（ファイル内のパスはリポジトリルートではなく指示ファイルを基準にしています）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントに関する要件はまだありません。このため、生成されたヘルパーにはドキュメントコメントがありません。次の手順で、TSDoc ドキュメントコメントとファイルコメントヘッダーのルールを追加します。

## 新しいリポジトリ標準を追加する

次に行う編集は、ドキュメントをコード内の TSDoc ドキュメントコメントとファイルレベルのコメントヘッダーとして管理するプロジェクト全体のルールを追加することです。このハーネスの後半で割り当てる Issue に取り組む際、クラウドエージェントはこのルールを参照します。

編集する前に、作業ブランチを準備します。クラウドエージェントは Issue の対象ブランチから指示を読み取りますが、編集内容は適切にコミットする必要があります。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` はプロジェクトレベルの情報を Copilot に提供するためのファイルです。リポジトリのコーディング標準を文書化し、コード補完の品質を向上させます。

1. `.github/copilot-instructions.md` を再度開きます。
2. **Code formatting requirements** セクションを探します。このセクションは 27 行目付近にあります。プロジェクトのコーディング標準は記載されていますが、コード内ドキュメントに関するルールがないことを確認します。これが、生成されたヘルパーにドキュメントコメントがなかった理由です。
3. ファイルコメントヘッダーと TSDoc ドキュメントコメントを追加するよう Copilot に指示するため、既存の標準の直下に次の Markdown 行を追加します。

   ```markdown
   - エクスポートするすべての関数には、その目的、パラメーター、戻り値を説明する TSDoc コメントを付けます。
   - インポートやコードの前に、ファイルの目的を説明するコメントブロックを追加します。
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリレベルのグローバルガイダンスには `.github/copilot-instructions.md` を、特定の言語、ファイル種類、タスク向けには `*.instructions.md` ファイルを作成できます。リポジトリレベルのファイルは、追加したドキュメントコメントルールなどのプロジェクト全体の標準を配置する適切な場所です。

## 指示の更新をコミット、プッシュ、マージする

クラウドエージェントは Issue の対象ブランチから指示ファイルを読み取ります。次の演習で Issue を Copilot に割り当てる際、Copilot は `main` からブランチを作成します。そのため、クラウドエージェントに反映させるには指示の変更を `main` に反映する必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` への pull request を作成してマージします。次の演習で割り当てる Issue を処理するとき、クラウドエージェントはこれらの指示を読み取ります。

> [!TIP]
> このワークショップでは `main` で直接作業してもかまいません。その場合は、ブランチ作成を省略して変更を直接 `main` にコミットできます。ブランチ手順は、実際のプロジェクトで行う一般的な作業を再現するためのものです。

## 概要と次のステップ

Copilot が指示ファイルからコンテキストを取得する仕組みを確認し、リポジトリ全体の新しい標準を `.github/copilot-instructions.md` に追加しました。このハーネスの残りの部分で生成されるクラウドエージェントの pull request に、この標準が反映されることを確認できます。

次に、[最初の Issue を Copilot クラウドエージェントに割り当てます][next-lesson]。

## リソース

- [GitHub Copilot カスタマイズ用の指示ファイル][instruction-files]
- [カスタム指示作成のベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示をより効果的にする 5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot - 指示ファイルとカスタムエージェントのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
