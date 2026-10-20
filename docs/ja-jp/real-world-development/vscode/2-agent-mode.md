---
title: "演習 2 - Copilot エージェント モードで新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションにごく簡単な変更を加えるだけでも、通常は複数のファイルを更新し、テストを実行する必要があります。開発者は、必要なファイルを見つけ、変更を加え、テストを実行し、問題をデバッグし、変更漏れのファイルを見つけて、さらに更新します。この作業は続いていきます。

こうした場合に役立つのが Copilot エージェント モードです。

Copilot エージェント モードは、IDE 内でより自律的に動作します。開発者と同じように既存のプロジェクト構造を調べ、必要な変更を加え、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェント モードで Web サイトに新機能を追加する方法を確認します。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる種類の作業に使います。エージェント モードは IDE 内で作業するため、すぐに結果を確認して対話できます。Cloud agent はチームのメンバーのように非同期で作業し、Issue やプルリクエストを通してやり取りします。

この演習では、次の方法を学びます。

- Copilot エージェント モードでプロジェクトを調べ、関係するファイルを特定し、複数ファイルの変更を調整する方法
- GitHub Copilot エージェント モードで UI とデータレイヤーにまたがる新機能を実装する方法
- コードベースにマージする前に、Copilot エージェント モードが生成した変更とテストをレビューする方法

## シナリオ

ゲームの一覧が増えてきたので、ユーザーがカテゴリや発行者で絞り込めるようにします。前の演習で発行者ヘルパーを追加しました。ここでは Copilot エージェント モードを使って、データレイヤー、UI、テストの残りの作業を完了します。

## Tailspin Toys Web サイトを実行する

変更を加える前に、Tailspin Toys Web サイトを操作して現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲームの一覧や詳細を表示します。単一の Astro アプリであり、ビルド時にページを静的 HTML として生成します。ページは frontmatter で Drizzle ORM を使い、ローカルの SQLite データベースに直接クエリします。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用するデータアクセス ヘルパーは `src/lib/` に配置し、インタラクティブな動作には標準 DOM API を使う小規模な Astro `<script>` を使用します。

### Web サイトを起動する

`npm run dev` タスクで Astro 開発サーバーを起動できます。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習の後、`main` ブランチにいるはずです。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. フィルタリング作業用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL を含むバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` のブラウザー ウィンドウを開くかどうかを尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> Codespace ターミナルで localhost URL のリンクを選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的にリダイレクトされます。これは Web サーバーをホストしている Codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトを起動したら、次の主な機能を確認します。

- **ホーム ページ**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行者、カテゴリなどの情報が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルは演習ごとに異なります。必要なときに各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して改善します。まずバックログを確認します。テンプレートからプロジェクトを設定すると Issue のバックログも自動的に作成されるため、GitHub Copilot に一覧を表示してもらいます。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** は自律的に計画を立て、複数ファイルを変更し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. 利用できるモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成には Claude Sonnet 4.5 を使用したため、比較的一貫した操作を行えるよう手順にも記載しています。ただし、LLM は確率的に動作するため、提案が例と異なる場合があります。これは正常であり、想定された動作です。

> [!NOTE]
> LLM の確率的な動作により、Copilot が別の MCP コマンドを使う場合がありますが、タスクを完了できるはずです。

3. 次のプロンプトを Copilot に送信し、Issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーにとって有用だと判断した順に項目を優先順位付けしています。

## 指示ファイルを確認する

コード生成エージェントを起動する前に、Copilot にコンテキストを提供する指示ファイルを確認します。Web サイトへの機能追加方法に関する情報を含む[ユーザー インターフェイス (UI) 指示][ui-instructions]を使用します。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の進め方に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要
   - コンポーネント設計、テスト容易性、アクセシビリティに関する原則
   - 次のようなファイルの種類別の指示ファイルへのリンク
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。指示を再利用し、複雑な内容を小さく分割し、例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリングの完成には、少なくとも次の 3 つの更新が必要です。

- データレイヤー (`src/lib/`) にフィルタリング ロジックを追加または調整する
- フィルタリングの動作をテストするテストを追加または更新する
- ゲーム一覧ページにフィルタリング UI を追加する

コードベースにマージする前にテストを実行し、成功する必要もあります。Copilot エージェント モードにこれらのタスクを任せます。

1. Copilot の現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルを選択する画面](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選ばれていることを確認します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** であることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、目的の機能に関係するファイルを見つけます。データレイヤーのヘルパー、UI、テストを特定して変更し、テストを実行します。

   ![Copilot がプロジェクト ファイルを調べる画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、変更、テストの実行など、複数のタスクを行います。タスクやコードベースの複雑さによっては数分かかります。作業中、コード エディターに **Keep** と **Undo** が表示されることがあります。完了すると、すべての変更に **Keep** または **Undo** が表示されるため、作業中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat でテストを実行してよいか確認するダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はコード生成とテストを繰り返し、タスクを完了してエラーがないと判断するまで作業します。

   ![Copilot エージェント モードの完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いたブラウザーに戻り、新しい機能を試します。
11. すべてが動作することを確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot エージェント モードで Tailspin Toys Web サイトに機能を追加する方法を学びました。次のことを行いました。

- GitHub Copilot エージェント モードで UI とデータレイヤー全体に新機能を実装しました。
- Copilot エージェント モードでプロジェクトを調べ、関係するファイルを特定し、変更を調整しました。
- コードベースにマージする前に、エージェント モードが生成した変更とテストをレビューしました。

次は [Playwright MCP サーバーで機能をテスト][next-lesson]し、プルリクエストを作成します。

### 追加の演習 — ページネーションを実装する

ゲームの一覧が増えると、ページネーションが必要になります。この演習で学んだ方法を使い、ページネーションを実装するよう Copilot に依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、ベスト プラクティスに従います。
- ユーザーがページ サイズを選べるようにするか、サイズを固定するかを決めます。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- GitHub Copilot にコンテキストを与えて変更を依頼し、反復する必要がある場合があります。Copilot と作業するときは、通常このように進めます。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
