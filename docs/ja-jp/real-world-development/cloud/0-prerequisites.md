---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot クラウドエージェントの演習を開始する前に、必要な環境を準備します。Tailspin Toys リポジトリのコピーを作成し、指示ファイルの編集とクラウドエージェントが生成した作業の確認に使用できる [Codespace][codespaces] を起動します。

## ラボリポジトリをセットアップする

作成するコード用のリポジトリのコピーを作成するには、[テンプレート][template-repository] からインスタンスを作成します。新しいインスタンスにはラボに必要なファイルがすべて含まれ、演習を進める間はこのリポジトリを使用します。

1. 新しいブラウザーウィンドウで、[Tailspin Toys テンプレート][tailspin-template] に移動します。
2. ラボリポジトリページで **Use this template** ボタンを選択し、リポジトリのコピーを作成します。続いて **Create a new repository** を選択します。

    ![このテンプレートを使用ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントの一環としてワークショップを受講する場合は、メンターから提供された手順に従います。それ以外の場合は、GitHub Copilot にアクセスできる Organization 内で新しいリポジトリを作成できます。

    ![リポジトリテンプレート設定の入力](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス（**organization-or-user-name/repository-name**）をメモしておきます。ラボの後半でこのパスを使用します。

## Codespace を作成する

次に、Codespace を使用してラボの演習を完了します。

[GitHub Codespaces][codespaces] はブラウザーで直接コードを作成、実行、デバッグできるクラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする完全な統合開発環境 (IDE) を提供します。

1. 新しく作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

    ![緑色の Code ボタンを選択](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい Codespace を作成](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、すべてのサービスを手動でインストールする場合よりもずっと短時間です。その間に、GitHub Copilot のほかの機能を確認できます。この操作については次の演習で扱います。

> [!CAUTION]
> 後の演習でこの Codespace に戻ります。それまではブラウザーのタブを開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] 内で実行できます。どちらでもスムーズに作業できるよう、必要な前提条件が環境にすべてインストールされています。ローカルで実行する場合は、複製したリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択します。VS Code は Codespace が使用するものと同じ dev container をビルドします。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 概要

ラボリポジトリのコピーを作成できました。また、Copilot クラウドエージェントと連携する際に使用する Codespace の作成も開始できました。

## 次のステップ

クラウドエージェントが従うカスタム指示を追加します。[演習 1 - カスタム指示][next-lesson] に進んでください。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の利用開始][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
