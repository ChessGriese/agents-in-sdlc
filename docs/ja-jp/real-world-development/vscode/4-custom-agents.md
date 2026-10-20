---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

[GitHub Copilot のカスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせて、専門の AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、特定の作業をより適切に行うための指示、ベスト プラクティス、コーディング パターン、分野固有の知識を提供します。チームの専門知識を再利用可能なエージェントにまとめられます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディング慣行に従うセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントを作成できます。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダーまたは全体に適用する `~/.copilot/agents` に置く Markdown ファイルで定義します。各ファイルには `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を定義する Markdown プロンプトが必要です。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には共通する部分があります。どちらも主に Markdown ファイルで定義され、AI に操作方法を指示します。簡潔に説明すると、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、作業を進めるときにスキルやほかのエージェントを調整できます。このラボでは、アクセシビリティ カスタム エージェントがガイドラインに照らしてサイトをレビューし、更新します。その作業の一環として、プルリクエスト ワークフロー スキルや、テストを実行および管理するスキルを呼び出すことができます。

> [!NOTE]
> カスタム エージェントの作成方法に唯一の正解はありません。AI を使うほかの作業と同様に、環境やシナリオに合う方法を試して調整してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のフィードバックによると、現在のダーク テーマはテキストと背景のコントラストが不十分で読みにくいと感じるユーザーがいます。このアクセシビリティの問題に対応するため、デザイン チームはユーザーがオンとオフを切り替えられるハイ コントラスト モードを依頼しました。

アクセシビリティは重要なので、できるだけ早く実装したいと考えています。カスタム エージェントを使って機能を生成します。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに作成されています。エージェントが Copilot を導く方法を確認します。

Codespace に戻り、ターミナルで `main` から新しいブランチを作成し、アクセシビリティ作業を始めます。フィルタリング PR は演習 3 のブランチに分けます。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` の各フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` を含む frontmatter が必要です。

3. 続くセクションを確認し、次の内容を読みます。
   - アクセシビリティに対応した Web サイトのコードを生成するときの主な責任
   - アクセシビリティのベスト プラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題や誤り

## Copilot Chat でカスタム エージェントを使う

VS Code では、`.github/agents` にあるカスタム エージェントが Copilot Chat ビュー下部のエージェント ドロップダウンに表示されます。カスタム エージェントを選ぶと、その指示とツールを使うようチャット セッションを設定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルは演習ごとに異なります。必要なときに各手順で説明します。

1. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていない場合は選択します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント ドロップダウンを選択します。現在のエージェントが表示されます。既定では **default** です。
3. 使用できるエージェントの一覧から **Accessibility agent** を選びます。
4. アクセシビリティ エージェントに次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat はコードベースを調べ、変更を提案してプロジェクト ファイルを編集します。各編集はファイル パスとレビュー用の差分とともにチャットに表示されます。
6. 編集が行われるたびに **Files changed** の表示が更新され、エージェントが変更したファイルを確認できます。

> [!NOTE]
> この作業には数分かかることがあります。Copilot はリポジトリを実際に変更し、Astro コンポーネントや CSS、関連するテストなどを編集します。

次の演習では、進行中の作業をレビューして案内します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた AI アシスタントである [GitHub Copilot のカスタム エージェント][custom-agents]を学びました。チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot の作業を導けます。

このレッスンでは、次のことを学びました。

- カスタム エージェントの定義方法
- Copilot Chat のエージェント モードでカスタム エージェントを使う方法

次は、[エージェントの作業を監視して案内][next-lesson]します。変更を確認し、同じセッションにライト モードの切り替えを追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

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
