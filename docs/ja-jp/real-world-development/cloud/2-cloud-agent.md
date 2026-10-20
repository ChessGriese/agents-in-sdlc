---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: カスタム指示][previous-lesson]

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業が終わると、人間の開発者がレビューできるように下書きのプルリクエストを作成します。面倒な作業を任せることで、開発者はより大きな課題に集中し、開発を加速できます。

このレッスンでは、Copilot cloud agent を使って次のことを行います。

- コード生成環境をカスタマイズします。
- 操作を安全に行うための方法を確認します。
- 範囲を明確にした Issue の重要性を学びます。
- Copilot に Issue を割り当てます。

## シナリオ

Tailspin Toys には、解消したい技術的負債があります。最初のサイトを作成した請負業者は、ドキュメントをほとんど残していません。まず、アプリケーションのエクスポート関数すべてに TSDoc の doc コメントを追加します。

さらに、デザイン チームはゲームを見つけやすくしたいと考えています。ゲームの詳細ページに、同じカテゴリのほかのタイトルを関連ゲームとして表示したいと考えています。まだ洗練された実装は必要なく、UX の受け入れテストに使えるものを求めています。現在は作業を妨げていますが、優先度の高い Issue がほかにあります。

こうした作業は後回しになりがちですが、Copilot cloud agent に割り当てるのに適しています。エージェントに非同期で作業させている間、別の作業に集中し、後で結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent][copilot-agents] は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。たとえば、[GitHub Issue を Copilot に割り当てる][assign-issue]と、Copilot は進捗を記録する下書きのプルリクエストを作成し、環境を準備してタスクに取り組みます。作業中でも完了後でも Copilot のセッションを確認できます。解決案のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。簡単に見える操作にも、詳しく調べると複雑さがあります。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペア プログラマーとして扱い、段階的に作業し、学び、試し、適応します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習で、カスタム指示と Copilot のガイド方法を学びました。カスタム指示を試したことがあれば、`.github/copilot-instructions.md` の効果を確認できたはずです。まだ確認していない場合は、cloud agent に作業を依頼する前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認しましょう。後で効果を見つけやすくなります。

GitHub の Web UI または起動中の Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクションを確認します。特に TypeScript の規約と TSDoc の doc コメントに関する要件に注目します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` frontmatter を確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、プルリクエストで TSDoc の doc コメント、TypeScript の規約、ファイル ヘッダーを確認します。後の演習で PR をレビューするときにも、この点を取り上げます。

## Copilot cloud agent の開発環境を設定する

コード作成では、必要なツールや依存関係を使えるように環境を準備します。Copilot にタスクを割り当てる場合も同じです。Copilot はソフトウェア エンジニアと同じように作業します。

Cloud agent は作業環境として [GitHub Actions][github-actions] を使います。作業開始前に実行する[セットアップ ワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成すると、必要な開発ツールや依存関係を使えるよう環境をカスタマイズできます。このラボでは、Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、単一 Astro アプリ用に移行してシードした SQLite データベースを利用できるよう、あらかじめ設定されています。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順で必要な*最小限のアクセス許可を設定します。Copilot は操作用の独自のトークンを受け取ります。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリを複製する場合は `contents: read` 権限が必要です。手順内で複製しない場合は、完了後に Copilot が自動的に複製します。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストに必要なデータを用意するため、ローカル SQLite データベースを移行してシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローに似ていますが、次の点が重要です。

- `copilot-setup-steps` という単一のジョブが含まれています。このジョブは Copilot がプルリクエストで作業する前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを設定すると、リポジトリの **Actions** タブから手動でワークフローを実行できます。Copilot の実行を待たずに、ワークフローが成功することを試せます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、情報が古い、または不足しているプロジェクトは少なくありません。これは対処されない技術的負債になりやすく、生産性を低下させ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成に役立つため、cloud agent に依頼するのに適したタスクです。バックグラウンドでドキュメントを生成し、後の演習で結果を確認します。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` などです。PR が一覧に表示されない場合は、少し待ってブラウザーを更新します。
12. 数分後に Copilot の todo リストが表示されます。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで、数分かかることがあります。Copilot は環境を準備し (前述のワークフローを実行します)、プロジェクトを分析して、問題への取り組み方を決めています。

13. todo リストと、実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された状態が表示されるまで、ウィンドウの再読み込みが必要な場合があります。

16. ライブ セッションをスクロールし、Copilot が問題に取り組む様子を確認します。コードを調査して状態を理解し、考えて適切な計画を決め、コードを作成する過程を確認できます。

この作業には数分かかることがあります。Copilot cloud agent の主な目的の 1 つは、非同期でタスクを実行し、別の作業に集中できるようにすることです。この機能を活用するため、もう 1 つのタスクを cloud agent に割り当て、その間にアプリケーションの機能を作成します。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点は、作業を分担できることです。デザイン チーム向けに関連ゲームのセクションを追加する作業に時間がかからないとしても、その間に別の作業を進められます。Copilot cloud agent に割り当てます。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

しばらくすると、Issue の最初のコメントに 👀 が表示され、Copilot が作業中であることを確認できます。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組み始めました。Copilot cloud agent はソフトウェア エンジニアと同じように動作するため、常に監視する必要はありません。完了後に確認します。次はカスタム エージェントを作成して使用します。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents]を学びました。cloud agent に Issue を割り当てると、非同期で作業させることができます。技術的負債の解消、新機能の作成、別のフレームワークへのコード移行などを依頼できます。

このレッスンでは、次のことを学びました。

- コード生成環境をカスタマイズする方法
- 操作を安全に行う方法
- 範囲を明確にした Issue の重要性
- Copilot への Issue の割り当て方法

cloud agent がバックグラウンドで作業している間に、次は[カスタム エージェントを作成して使用][next-lesson]します。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前の演習で扱ったカスタム指示も使用します。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
