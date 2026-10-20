---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使うときはコンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。特に重要なのが[指示ファイル][instruction-files]です。コード生成についてのガイダンスを提供できます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス別の指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 現在の指示を使ってフィルタリング用の最初のデータ スライス (発行者ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加します。
- 同じプロンプトを再送信し、生成されたコードに新しい標準が反映されることを確認します。
- 指示の更新とフィルタリングの変更を `main` にコミットし、次の演習で Copilot が新しいガイダンスを使えるようにします。

> [!CAUTION]
> 生成されたコードが指定した標準に沿わない場合があります。Copilot の動作は確率的です。この演習では、指示を更新した後に動作が変わる*傾向*を確認します。出力が 1 文字ずつ一致することを期待するものではありません。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発慣行に関するガイドラインと要件があります。

- データレイヤーには必ずユニット テストを追加します。
- UI はダーク モードで、モダンな外観にします。
- コード内のドキュメントには TSDoc の doc コメントを使います。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って、Copilot がこれらの慣行に沿って作業するために必要な情報を提供します。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件を Copilot に伝え、求める提案やコードを得やすくなります。規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定のファイルやタスクだけに適用する指示を作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリに対する**すべての**リクエストで Copilot に送られる単一の指示ファイルです。技術スタック、構築するものの概要、全般的なベスト プラクティスなど、ほとんどのチャットや CLI リクエストに関係するプロジェクト情報を記載します。
- `.github/instructions/*.instructions.md`: 特定のタスクやファイルの種類に関する指示を記載します。TypeScript や Astro などの言語、UI コンポーネントや新しいテストの作成などを対象にできます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われます。コード補完や次の編集の提案には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と、`applyTo` frontmatter を含む `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの規格の指示ファイルにも対応しています。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成方法を網羅することはこの演習の範囲外ですが、サンプル プロジェクトには代表的な方法が示されています。

- `copilot-instructions.md` には、構築するものの説明、プロジェクト構造、全般的なコーディング標準など、プロジェクト レベルの情報を記載します。
- `*.instructions.md` には、ユニット テスト、Astro コンポーネント、データレイヤーなど、ファイルの種類やタスクに固有の指示を記載します。
- 自然な言葉を使い、明確なガイダンスとコード例を示します。

指示ファイルの作成方法に唯一の正解はありません。AI の使い方と同様に、試しながらプロジェクトに合う方法を見つけます。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、充実した指示ファイルを用意することをお勧めします。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの指示があります。
>
> Copilot に指示ファイルの作成を依頼することもできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` などを使います。利用する環境のレッスンで具体的な手順を確認してください。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタム エージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、基本の `copilot-instructions.md` と、さまざまなタスク用の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** の指示にも注目してください。これらは Copilot を使ったすべての作業に関係します。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドには、指示の対象となるファイルをリポジトリ ルートからの glob で指定します。たとえば `**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトでユニット テストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾を開きます。`unit-tests.instructions.md` など、ほかの指示ファイルやプロジェクト内のファイルへのリンクがあります。このように大きな指示を再利用しやすいファイルに分け、参考例を示せます。記載されたパスはリポジトリ ルートではなく、この指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc の doc コメントとファイル ヘッダーを追加する規則を設定します。

## 変更用のブランチを作成する

ベスト プラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する*前*に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルは演習ごとに異なります。必要なときに各手順で説明します。

カスタム指示の影響を確認するため、現在の指示ファイルを使ってプロンプトを送信します。その後、指示を更新して、同じプロンプトを再送信した結果を比較します。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを読み取るようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を示します。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べて変更を適用します。ヘルパーとテストなど、複数のファイルが変更される場合があります。
6. 提案されたヘルパーが、`db` クライアントを最初の引数として受け取り、発行者の型付き配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約によるものです。
7. 提案されたコードには TSDoc の doc コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc コメントを追加する場合があります。その場合も問題ありません。指示を更新した後に動作の一貫性が改善することが、この演習の目的です。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。コード提案を改善するため、コーディング標準に新しい規則を追加します。

1. `.github/copilot-instructions.md` を開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。既存の標準には、コード内のドキュメントに関する規則がありません。
3. 既存の標準の直後に、次の Markdown を追加し、ファイル ヘッダーと TSDoc の doc コメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、`.github/copilot-instructions.md` はリポジトリ全体に、`*.instructions.md` は特定の言語、ファイルの種類、タスクに適用できます。ここで追加した doc コメントの規則のようなプロジェクト全体の標準は、リポジトリ レベルのファイルに記載します。

## 同じプロンプトを再送信して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` をもう一度開き、Copilot が適切なファイルに集中できるようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 今度は、提案されたファイルの先頭に次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準に従うよう Copilot を導き、次の演習で使う実際のコードに適用できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルも、ほかのリポジトリ資産と同じようにソース管理で管理します。ブランチをコミットしてプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. ターミナルで `git status` を実行し、指示の更新とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. ターミナルで指示ファイルとヘルパーの基盤となる変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示される場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の Issue に関連付けたプルリクエストを作成します。手動で操作する代わりに、プロジェクトに接続済みの GitHub ツールを使って Copilot に依頼できます。Issue を検索し、その Issue をクローズする PR を作成してからマージするよう依頼します。

> [!NOTE]
> この演習では初めて Copilot に GitHub 操作を依頼します。プロジェクト テンプレートで GitHub ツールがワークスペースに設定済みのため、追加のセットアップは必要ありません。Copilot が最初にツールを使うとき、VS Code で GitHub へのサインインを求められる場合があります。画面の手順に従ってください。後の演習では、この接続が機能する仕組み (Model Context Protocol) を学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントの選択で **Agent** が選ばれていることを確認します。
3. 指示ファイルの更新と、新しい発行者ヘルパーを説明する現在のブランチから PR を作成し、関連する Issue をクローズするよう Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue の検索や PR の作成などを行います。
5. Copilot が代わりに GitHub 操作を行う許可を求めたら、コマンドを確認し、適切であれば **Allow** を選択します。
6. PR の作成後、PR を `main` にマージしてローカルのブランチを `main` に戻し、最新のコードを取得するよう Copilot に依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub 操作やシェル コマンドを実行する許可を求められたら、内容を確認して適切であれば **Allow** を選択します。

GitHub Copilot の支援を使って、プルリクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを得る方法を確認し、VS Code の Copilot Chat で次のことを行いました。

- 既存の指示を使ってコード生成のプロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体へ適用する標準を追加しました。
- 同じプロンプトを再送信し、提案されたコードに新しい標準が反映されることを確認しました。
- 指示の更新とヘルパーの基盤を `main` にコミットし、次の演習で使えるようにしました。

次は[エージェント モード][next-lesson]で指示を活用し、コードベース全体にわたる新機能を追加します。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
