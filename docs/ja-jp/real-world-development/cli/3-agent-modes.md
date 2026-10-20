---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で方針を確認し、Autopilot で Issue に基づくフィルタリングを実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に、小さな機能をプロジェクトに追加しました。より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は組織の既存のフローに沿って動作するため、適切なものを適切な方法で構築できます。このレッスンから数回にわたって、Issue を使った機能開発、コードの検証、動作確認、プロジェクトへのマージという、一般的なエージェント主導の開発プロセスを進めます。

> [!NOTE]
> 機能のワークフローを続ける間は、同じ会話とブランチを使用します。通常は作業するファイルの種類ごとにブランチや PR を分けますが、ここでは主要な概念に集中するため、手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub Issue から新しい Copilot 会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

この機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加し、QA エージェントを作成して、機能 PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がカテゴリや発行者でゲームを絞り込める必要があります。バックログ Issue には機能が記載されていますが、カテゴリの組み合わせなど、コーディング前に合意すべき詳細があります。Plan モードで判断を整理してから、範囲を限定した実装を Autopilot に任せます。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本は変わりません。むしろ、これまで以上に重要になります。多くの開発者は、次のような手順で作業します。

1. 必要な作業を記載した Issue を作成します。
2. 実装内容を計画します。
3. コードを作成してレビューします。
4. テストを実行して検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、変更をマージします。

> [!NOTE]
> チームや組織によって詳細は異なりますが、一般的には上記の流れに沿って作業します。

この標準的な手順に従うことで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じ確認プロセスを通るようにします。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントと共同で作業します。エージェントは変更案を提示し、次に進む前に入力を待ちます。
- **Plan**: エージェントが最初に計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: エージェントがコードを書き、テストを実行して、タスクの完了まで自律的に反復します。

最初に Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から開始する

フィルタリングに取り組む前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止中の場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルタリング用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと GitHub の Issue やプルリクエストをコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てると、機能の実装やタスクの実行を Copilot に依頼する前に、その方法を決められます。複雑な作業では、時間をかけて計画することをお勧めします。Plan モードに切り替え、計画の作成を Copilot に依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適な方法を検討して計画を作成します。

3. フィルタリングの動作について Copilot が質問した場合は、自分の希望に沿って回答します。正解は 1 つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 計画全体を確認します。
6. 決定した内容と異なる箇所があれば、修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に移ります。Autopilot を使って Copilot に作業を任せます。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みバージョンで同様の名前が付いた選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、実装がすぐに始まる場合があります。計画を先にレビューしてください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェックが完了したと判断する前にセットアップの問題を解決してください。

## 実装をレビューして検証する

コードを生成したら、ほかのコードと同様にマージ前にレビューします。コードを確認し、サイトを起動して、動作も確認します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. 結果を Issue と計画時の決定事項に照らし合わせます。
4. レビューが終わったら、<kbd>Esc</kbd> を押して差分表示を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新しい機能を試す

コードが問題なく見えても、実際に動作するとは限りません。前と同じようにアプリを起動し、Codespaces から転送されたポートを使ってサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能と表示されたら、**Open in Browser** を選択します。
3. カテゴリ、発行者、計画時に決めた組み合わせで絞り込みを試します。
4. リセット時と結果が空のときの動作が、Issue と決定事項に合っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

複数の会話モードを使って機能を実装し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から新しい Copilot 会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、[カスタム指示を使って][next-lesson]、コードが文書化された開発慣行に従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI のコマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
