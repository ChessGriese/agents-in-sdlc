---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

[GitHub Copilot のカスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせて、専門の AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、特定の作業をより適切に行うための指示、ベスト プラクティス、コーディング パターン、分野固有の知識を提供します。チームの専門知識を再利用可能なエージェントにまとめられます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディング慣行に従うセキュリティ エージェント、一貫したテスト パターンを保つテスト エージェントを作成できます。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダーまたは全体に適用する `~/.copilot/agents` に置く Markdown ファイルで定義します。各ファイルには `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を定義する Markdown プロンプトが必要です。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には共通する部分があります。どちらも主に Markdown ファイルで定義され、AI に操作方法を指示します。簡潔に説明すると、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントには独自のコンテキスト ウィンドウがあります。作業を進める際に、スキルやほかのエージェントを調整できます。このラボのアクセシビリティ エージェントは、アクセシビリティ ガイドラインに照らしてサイトをレビューし、更新します。その作業の一環で、プルリクエスト ワークフローのスキルや、テストを実行、管理するスキルを呼び出すことができます。

> [!NOTE]
> カスタム エージェントに唯一の正しい作成方法はありません。AI を使うほかの作業と同じように、環境やシナリオに合うよう試しながら調整してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

このレッスンでは、カスタム エージェントを使って次のことを学びます。

- カスタム エージェントの定義方法
- エージェントへのタスクの割り当て方法

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のフィードバックによると、現在のダーク テーマはテキストと背景のコントラストが不十分で読みにくいと感じるユーザーがいます。このアクセシビリティの問題に対応するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードの実装を依頼しました。

アクセシビリティは重要なので、できるだけ早く実装したいと考えています。カスタム エージェントを使って機能を生成します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはあらかじめ作成されています。エージェントの指示を確認します。

Codespace に戻り、アクセシビリティ カスタム エージェント ファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` の各フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションを確認し、次の内容を読みます。
   - アクセシビリティに対応した Web サイトのコードを生成するときの主な責任
   - アクセシビリティのベスト プラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題や誤り

## Issue を作成して割り当てる

Mission control は、環境内のすべてのエージェント作業を管理する場所です。Copilot cloud agent にタスクを割り当て、進行状況を監視し、追加の指示を出したり作業の方向を変えたりできます。まず、ハイ コントラスト モードの作成を Copilot に依頼します。

1. リポジトリを開きます。
2. **issues** タブを選択します。
3. **New issue** を選択して新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![カスタム エージェントとアクセシビリティ エージェントが選択された cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドでタスクに取り組みます。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた AI アシスタントである [GitHub Copilot のカスタム エージェント][custom-agents]を学びました。チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot の作業を導けます。

このレッスンでは、次のことを学びました。

- カスタム エージェントの定義方法
- エージェントへのタスクの割り当て方法

Copilot がハイ コントラスト モードを実装している間に、mission control からエージェント セッションを[監視して案内][next-lesson]します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使用する準備][org-custom-agents]
- [エンタープライズでカスタム エージェントを使用する準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
