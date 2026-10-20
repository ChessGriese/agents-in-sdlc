---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使用する"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルタリング機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、必要な作業があります。これまでは、手動でコードの動作を確認し、指示ファイルを使って標準に従うようにしました。では、テストや lint、継続的インテグレーション (CI) のほかの部分はどうでしょうか。

こうしたタスクには **エージェント スキル**が適しています。スキルを使うと、これらの操作を正しく実行する方法を Copilot に伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、ユニット テスト、lint、型チェックに `quality-checks` スキルを使用します。チームは結果を読みやすくするため、レポートの改善を希望しています。

## 指示、スクリプト、リソース

エージェント スキルは、エージェントが必要なときに読み込める再利用可能なタスクの指示、実行可能なスクリプト、補助リソースをまとめます。基本的な構成は、スキル名と同じ名前のフォルダーに置いた `SKILL.md` ファイルです。Markdown にはスキルの名前と説明を示す frontmatter、概要、呼び出すタイミングに関するガイダンスを記載します。スキル フォルダーに、呼び出し時に使うスクリプトやその他のリソースを含むサブフォルダーを追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルはプロジェクトにある既存のコマンドを使うため、`SKILL.md` のみで構成されています。

スキルは、チームで共有、再利用できるリポジトリ資産として `.github/skills` に配置するか、ユーザーのスキル フォルダー `~/.copilot/skills` に配置できます。

## スキルを確認する

Tailspin Toys チームがユニット テスト、lint、型チェックに使う `quality-checks` スキルを確認します。

1. Codespace に戻ります。Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。説明は、Copilot がスキルをいつ呼び出すか判断するために使います。
3. 指示を読み、テストと lint の手順をどのように案内しているか確認します。
4. スキルには **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

Copilot CLI では、スキルを直接呼び出したり、自然な言葉で実行を依頼したりできます。Copilot に 3 つのチェックを実行するよう依頼します。

1. Interactive モードでフィルタリングの会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

何を実行し、成功したかどうか、ツールがどのような結果を返したかが分かるレポートを作成します。スキルを更新します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を実際に試します。Copilot CLI では、会話を再起動しなくても編集したスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認し、最初の結果と比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして使用しました。このレッスンでは、次のことを行いました。

- ユニット テスト、lint、型チェック用の `quality-checks` スキルを確認しました。
- 結果の形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、フィルタリング機能とともに機能 PR に含まれます。次は、[Playwright MCP サーバーを使って][next-lesson]Copilot がサイトを直接操作できるようにします。

## そのほかのスキルの例

コミュニティの次の例は参考資料です。このレッスンで追加の作業を行う必要はありません。

- [Agent Skills の仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
