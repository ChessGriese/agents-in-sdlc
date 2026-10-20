---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

エージェント モードでフィルタリング機能を作成しました。プルリクエストを開く前に、ブラウザーで動作を確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と MCP サーバーが Copilot に新しいツールを追加する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に MCP サーバーを使わせ、ブラウザーでフィルタリング機能を手動テストします。
- ブランチを公開し、フィルタリングのプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェント モードは、エディターの外にアクセスできると、さらに強力になります。Model Context Protocol (MCP) は、Copilot が外部のツールやサービスと通信するための標準的な方法です。

![エージェント モードの内部と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントが外部のツールやサービスと通信できます。MCP を通じて、エージェントはリソースを使って最新情報にアクセスし、ツールを使ってユーザーに代わって操作できます。

MCP サーバーは、AI エージェントと外部のツールやサービスとの橋渡しをします。既存の API や npm パッケージなどのローカル ツールとの通信を管理し、それぞれのサーバーからエージェントが使えるツールとリソースを提供します。

よく使われる MCP サーバーの例を紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API を利用できます。リポジトリの作成や更新、Issue やプルリクエストの管理などを行えます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供します。Web ページの移動、フォームへの入力、ボタンの選択などを行えます。

ほかにもさまざまなツールやリソースを利用できる MCP サーバーがあります。GitHub の [MCP レジストリ](https://github.com/mcp)で、エコシステムのリソースを見つけられます。

> [!CAUTION]
> MCP サーバーはほかの依存関係と同じように扱ってください。利用する前にソース コードをよく確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できるサーバーのみを使い、機密性の高いリソースや操作へのアクセス許可には注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで使用できる MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` エントリはプロジェクト テンプレートに含まれているため、前の演習で Copilot が Issue のバックログを読み取ることができました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルへのインストールは不要です。VS Code は HTTP で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと Copilot がブラウザーを操作できます。機能のテストに利用します。

1. `.vscode/mcp.json` で、`github` の横に `playwright` エントリを追加します。ファイルは次のようになります。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモート サーバーの `github` とは異なり、`playwright` は**ローカル** サーバーです。VS Code は `npx @playwright/mcp@latest` を実行してサーバーを起動します。`--headless` フラグを付けると、画面に表示せずにブラウザーを実行します。Codespace には表示用のデスクトップ環境がないため、このオプションが必要です。

> [!NOTE]
> Tailspin Toys プロジェクトではエンドツーエンド テストに Playwright を使用しているため、通常はブラウザーがインストール済みです。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行して再度試すよう依頼します。

## サーバーを起動して信頼する

VS Code は、必要なときに MCP サーバーを起動します。Copilot が初めてサーバーのツールを使うとき、VS Code はサーバーを起動し、信頼するかどうかを確認します。

1. `.vscode/mcp.json` で、`playwright` エントリの上に **Start** アクションが表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code がサーバーを信頼するか尋ねたら、構成を確認して信頼するよう選択し、サーバーを起動します。

手動での起動は任意です。省略した場合、次の手順で Copilot がツールを初めて使うときに VS Code がサーバーを起動し、信頼を確認します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを開いて確認する代わりに、エージェントがページを開き、移動してフィルターを適用し、結果を読み取って報告します。会話を続けながら、機能が想定どおり動作するか確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。エージェントは、ボタン、リンク、リスト項目など、ラベルの付いた構造化要素に基づいて処理します。支援技術と同様にページを認識するため、簡単な機能確認がアクセシビリティの簡易確認にもなります。

サーバーを接続し、アプリを起動した状態で、作成したフィルタリング機能を Copilot に試させます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーでブラウザーを起動し、各手順を実行して結果を報告します。報告を Issue の受け入れ条件と照らし合わせます。想定と異なる箇所があれば、質問を続けたり、プルリクエストを開く前にコード修正を依頼したりします。

> [!NOTE]
> テストを実行するにはアプリが `http://localhost:4321` で起動している必要があります。開発サーバーを停止した場合は、プロンプトを送信する前に再起動します。Copilot が Playwright MCP サーバーを初めて使うとき、ブラウザーをダウンロードする場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行して再度試すよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したので、チームがレビューできるようにプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルを開き、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンでコミット メッセージを生成します。

   ![Copilot が行った変更を表示する Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンド ライン インターフェイス (CLI) など、複数の方法で PR を作成できます。ここでは GitHub Copilot に PR を作成してもらいます。関係する Issue を見つけ、関連付けた PR を作成できます。

1. Copilot Chat パネルを開き、**New Chat** を選択して新しいセッションを開始します。
2. Copilot が GitHub ツールを使えるよう、エージェントのドロップダウンで **Agent** が選択されていることを確認します。
3. Copilot に PR を作成するよう依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選び、情報の収集や操作を許可します。GitHub ツールを初めて使うときに GitHub 認証を求められる場合は、画面の指示に従います。
5. Copilot が Issue を検索し、適切なものを見つけて PR を作成する様子を確認します。
6. Copilot が作成したリンクを選択して PR をレビューします。まだ**マージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーで Copilot に新しいツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーでフィルタリング機能を手動テストしました。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成しました。

MCP サーバーを使って機能を**テスト**しましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は、アクセシビリティ レビューなど、範囲を絞ったタスクを効率よく行う[カスタム エージェントを作成][next-lesson]します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP レジストリ][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
