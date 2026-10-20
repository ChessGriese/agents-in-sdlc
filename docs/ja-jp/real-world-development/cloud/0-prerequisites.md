---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要なものを準備します。コードを作成するための Tailspin Toys リポジトリをコピーし、[Codespace][codespaces] を作成します。Codespace では、指示ファイルの編集や cloud agent が作成した内容のレビューを行います。

## ラボ リポジトリをセットアップする

演習で使うコードのコピーを作成するため、[テンプレート][template-repository]からリポジトリを作成します。新しいリポジトリにはラボに必要なファイルがすべて含まれます。演習を通して、このリポジトリを使用します。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. ラボ リポジトリのページで **Use this template** を選択し、独自のリポジトリを作成します。続けて **Create a new repository** を選択します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が進行するイベントでワークショップに参加する場合は、メンターの案内に従ってください。それ以外の場合は、GitHub Copilot にアクセスできる組織に新しいリポジトリを作成できます。

    ![リポジトリ テンプレートの設定を入力](../../../_images/ex0-repository-settings.png)

4. 後の手順で参照するため、作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。

## Codespace を作成する

次に、Codespace を使ってラボの演習に取り組みます。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードを作成、実行、デバッグできるクラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールに対応した本格的な IDE を利用できます。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい Codespace を作成](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、すべてのサービスを手動でインストールするよりはずっと早く完了します。その間に、これから扱う GitHub Copilot の機能を確認してみましょう。

> [!CAUTION]
> 今後の演習で Codespace を再び使います。今はブラウザーのタブを閉じずに開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で実行するように設計されています。どちらも演習に必要なものがあらかじめインストールされています。ローカルで実行する場合は、複製したリポジトリを VS Code で開き、メッセージが表示されたら **Reopen in Container** を選択します。Codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ラボ リポジトリのコピーを作成し、Copilot cloud agent と一緒に作業するときに使う Codespace の作成を開始しました。

## 次のステップ

次は、cloud agent が従うカスタム指示を追加します。[演習 1 - カスタム指示][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の概要][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
