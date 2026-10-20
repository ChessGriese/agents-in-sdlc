---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときはコンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に何をしてほしいかだけでなく、どのように作業してほしいかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス別の指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では変更前後のプロンプトを実行しません。Copilot cloud agent は GitHub Issue に対して非同期に動作するため、影響をリアルタイムに並べて示すのは困難です。このハーネスの後半で、cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの効果を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発慣行に関するガイドラインと要件があります。

- データレイヤーには必ずユニット テストを追加します。
- UI はダーク モードで、モダンな外観にします。
- コードのドキュメントには TSDoc の doc コメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って、Copilot がこれらの慣行に沿って作業するために必要な情報を提供します。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件を Copilot に伝え、求めるコードや提案を得やすくなります。規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体に適用する指示と、特定のファイルやタスクだけに適用する指示を作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送られる単一の指示ファイルです。技術スタック、構築するものの概要、全般的なガイダンスなど、多くのチャットや CLI リクエストに関係するプロジェクト情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類向けに作成できます。TypeScript などの言語や、UI コンポーネントやユニット テストの作成といったタスク向けのガイドラインを記載します。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われます。コード補完や次の編集の提案には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と、`applyTo` frontmatter が設定された `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの規格の指示ファイルにも対応しています。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成方法を網羅することはこの演習の範囲外ですが、サンプル プロジェクトには代表的な方法が示されています。概要は次のとおりです。

- `copilot-instructions.md` には、構築するもの、プロジェクトの構造、全般的なコーディング標準など、プロジェクト レベルの情報を記載します。
- `*.instructions.md` には、ユニット テスト、Astro コンポーネント、データレイヤーなど、ファイルの種類やタスクに固有の指示を記載します。
- 自然な言葉を使って、明確なガイダンスとコード例を示します。

指示ファイルの書き方に決まった正解はありません。AI を使う場合と同じく、試して調整しながら環境やシナリオに合う方法を見つけます。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、充実した指示ファイルを用意することをお勧めします。このプロジェクトでは、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルを確認できます。
>
> Copilot に指示ファイルを作成させることもできます。利用する環境によって操作は異なります。たとえば、VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使用します。該当するレッスンで手順を説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタム エージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、基本の `copilot-instructions.md` と、さまざまなタスク用の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** の指示にも注目してください。これらは Copilot を使ったすべての作業に関係します。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドでは、指示の対象になるファイルを glob で指定します。ここでは、`**/*.test.ts` などに一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトでユニット テストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾を開きます。`unit-tests.instructions.md` などのほかの指示ファイルや、プロジェクト内のファイルへのリンクがあります。こうすると、大きな指示セットを小さく再利用しやすいファイルに分割し、参考例を示せます。ここに記載されたパスはリポジトリ ルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc の doc コメントとファイル ヘッダーを追加する規則を加えます。

## リポジトリ全体に適用する標準を追加する

ここで行う編集は 1 つだけです。コード内に TSDoc の doc コメントとファイル レベルのコメント ヘッダーを記載するよう、プロジェクト全体に適用する規則を追加します。後の演習で cloud agent に Issue を割り当てると、この指示が反映されます。

編集前に、新しいブランチを作成します。cloud agent は Issue の対象ブランチにある指示を読み取りますが、変更をきれいにコミットするため、ここではブランチを使います。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前の手順で説明したように、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。コード提案を改善するため、TSDoc の doc コメントとファイル ヘッダーを追加する規則をコーディング標準に記載します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する規則はまだありません。そのため、生成されたヘルパーには doc コメントがありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加します。これにより、Copilot にファイルのコメント ヘッダーと TSDoc の doc コメントを追加するよう指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体に適用する `.github/copilot-instructions.md` と、特定の言語、ファイルの種類、タスクに適用する `*.instructions.md` ファイルを使えます。ここで追加した doc コメントの規則のようなプロジェクト全体の標準は、リポジトリ レベルのファイルに記載します。

## 指示の更新をコミット、プッシュ、マージする

cloud agent は、Issue が対象とするブランチから指示ファイルを読み込みます。次の演習で Copilot に Issue を割り当てるとき、Copilot は `main` からブランチを作成するため、指示が反映されるよう変更を `main` にマージします。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。次の演習で Issue に割り当てたとき、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチを作成せず、`main` に直接コミットしてもかまいません。ここでは、実際のプロジェクトの手順に近づけるため、ブランチを使用しています。

## まとめと次のステップ

このプロジェクトの指示ファイルを Copilot が使用する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体へ適用する標準を追加しました。この標準が、このハーネスの後半で cloud agent が作成するプルリクエストに反映されます。

次は、Copilot cloud agent に[最初の Issue を割り当て][next-lesson]ます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
