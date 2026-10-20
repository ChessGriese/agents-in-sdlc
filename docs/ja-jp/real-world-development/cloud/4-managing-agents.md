---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の数回の演習で、ユーザー エクスペリエンスの改善と機能追加を目的とした 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業するよう設計されていますが、タスクを監視することも重要です。

GitHub.com の[エージェント ページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールが複数あります。この mission control では、オープンなプルリクエスト (PR) があるエージェントのタスクを一覧で確認し、実行内容を調べ、進行中のセッションに追加の指示を与えられます。

このレッスンでは、次のことを行います。

- エージェント ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

エージェントにハイ コントラスト モードを依頼した後、チームはライト モードも追加することにしました。サイトのスタイルを更新して切り替え機能を追加しているところなので、合わせて実装するのが適切だと考えています。ハイ コントラストに加えてライト モードも実装するよう、エージェントに追加の指示を出します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左ペインに表示されるタスク一覧を確認します。割り当てた次のタスクが表示されます。
   - コードのドキュメントを更新する
   - ゲームを変更する API を生成する
   - Web サイトにハイ コントラスト モードを追加する
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。次のような操作が表示されます。
   - リポジトリからコードをチェックアウトする
   - Copilot の作業環境を作成する
   - MCP サーバーをセットアップする
   - タスクを完了するために各種の手順を実行する

> [!NOTE]
> 表示される手順は、Copilot の作業状況やアプローチによって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを監視できます。

## cloud agent に追加の指示を出す

アクティブなタスクを確認したので、ハイ コントラスト モードの作業中にライト モードも追加するよう Copilot に依頼します。

1. ハイ コントラスト モードを追加しているセッションを選択します。正確なタイトルは、Copilot が使った名前や作業状況によって異なります。

    ![mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業を開始したことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![agents ページの cloud agent タスクで、Steer active session while Copilot is working ダイアログを強調表示した画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が指示を受け取り、作業に組み込む様子を確認します。

## Copilot の作業を待つ

前回と同様に、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点のステップを完了した後、新しい依頼を作業に反映します。

これまでの演習と同じく、完了まで数分かかります。その間に、ここまで学んだ内容を振り返ってみましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視して案内するための中心的な場所である Copilot agents ページを確認しました。ここでは、進行中および完了したタスクを追跡し、実行内容をレビューし、進行中のタスクに追加の指示を出せます。

このレッスンでは、次のことを行いました。

- mission control とエージェント ページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、次は Copilot が作成したプルリクエストを[改善][next-lesson]します。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
