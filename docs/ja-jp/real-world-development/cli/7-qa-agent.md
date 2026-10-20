---
title: "レッスン 7 - QA エージェントを作成して使用する"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルを使って自動チェックを実行し、Playwright MCP を使ってブラウザーでフィルタリングの操作を確認しました。次は、役割を明確に定めたカスタム エージェントにこれらの機能をまとめ、QA プロセスを再利用できるようにします。

このレッスンでは、次のことを行います。

- カスタム エージェントが指示、スキル、MCP ツールと連携する仕組みを確認します。
- 再利用できる品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルタリング Issue に照らして結果を確認します。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト範囲、ブラウザーでの動作を一貫してレビューしたいと考えています。カスタム エージェントを使うと、この QA プロセスを調整して、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の専門エージェントです。目的、指示、使用できるツールを記載します。このワークショップでは、`.github/agents/qa.agent.md` で QA の役割を定義し、Copilot CLI で選択します。

これまでに使用したカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を示します。`quality-checks` スキルは繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、要件を評価して結果を報告するために、Copilot がこれらの機能をどう使うかを指定します。ほかの機能の代わりになるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能 PR を作成する前に、再利用できる QA プロファイルの作成を Copilot に依頼します。実行するチェックと、守るべき制約の両方をプロファイルに定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、カスタム エージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA ワークフローと制約を正しく設定したか確認します。これにより、検証だけを依頼したのにエージェントが機能を変更するといった事態を防ぎます。

1. `/diff` を入力して `.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は省略できますが、指定するとエージェントの表示名が明確になります。
3. プロファイルの指示を読み、QA が要件を起点にして、リポジトリの指示に従い、`quality-checks` スキルを実行し、Playwright MCP を使用することを確認します。
4. QA が根拠を示して結果を報告し、実装コードを変更する前に確認を求め、変更をコミットせず PR も開かないことを確認します。
5. 必要な役割や制約が不足していたら、続行する前に既定のエージェントへ修正を依頼します。

## Issue に照らして QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成したら、同じフィルタリングの会話を再開し、QA を選択して、Issue と計画時の決定事項を利用できるようにします。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、使用可能なエージェントの一覧に表示されないことがあります。上記のコマンドで有効にできます。

2. 次のプロンプトで QA エージェントに機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 完了したら、エージェントのレポートを読みます。

## まとめと次のステップ

ワークフローに再利用できる専門の役割を追加し、その作業をレビューしました。このレッスンでは、次のことを行いました。

- カスタム エージェントが指示、スキル、MCP ツールと連携する仕組みを確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルタリング Issue に照らして結果を確認しました。

機能の実装、スキルの更新、QA プロファイル、テスト、検証レポートを用意できました。次は、[機能 PR を作成し、Agent Merge を使用][next-lesson]します。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
