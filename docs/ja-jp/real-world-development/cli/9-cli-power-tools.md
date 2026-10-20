---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、クラウドへの任意の委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

ほかの CLI ツールと同様に、GitHub Copilot CLI にも多くのスラッシュ コマンドがあります。これらのコマンドから、高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。次は、ほかの便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` を使い、利用できるモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、クラウド エージェントへの委任に使える任意のコマンドを確認します。

## シナリオ

CLI の基本的なワークフローが完了しました。ここでは、コンテキストの管理、モデルの切り替え、セッションの共有などの機能を確認します。また、必要に応じて [Copilot cloud agent][about-cloud-agent] に作業を委任できます。

## Copilot CLI のコンテキストを確認する

大きなタスクや複雑なタスクに取り組むと、モデルの最大コンテキスト ウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュ コマンドを使って状態を確認したり、自分で圧縮したりできます。

1. Codespace に戻り、Copilot CLI がまだ開いていない場合は、リポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域ごとのコンテキストの内訳を確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話がまだ短い場合、大きな変化はないことがあります。

> [!NOTE]
> コンテキスト ウィンドウがいっぱいになると、Copilot CLI は自動的にコンテキストを圧縮します。実行タイミングを自分で決める場合は `/compact` を使用してください。関係のない別のタスクを開始する場合は、`/clear` または `/new` で新しい会話に切り替えます。

## モデルを選択する

モデルごとに得意なことが異なり、開発者によって好みも異なります。Copilot CLI では、利用できるモデルを一覧表示して選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用できるモデルと使用情報を確認します。
3. 現在のモデルを使用し続けるか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、学んだことを共有すると、AI ツールの活用方法を改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートし、共有可能なリンクを作成し、GitHub gist として公開できます。

1. `/help` を入力し、インストール済みバージョンで利用できる `/share` のオプションを確認します。
2. セッションを共有する場合は、ローカルの Markdown 形式で保存する `/share file` など、目的に合う方法を選びます。
3. エクスポートした内容を他の人に送信したり公開したりする前にレビューします。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有する予定のないリポジトリや会話の内容は公開しないでください。

## 任意: 作業を拡大または委任する

ワークショップの主要な内容は完了しました。Copilot CLI には、大きなタスクに使えるコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割し、並列実行できます。
- `/worktree` は、別の作業用に独立した Git worktree を作成できます。
- `/delegate` は、Copilot cloud agent にタスクを送り、非同期で作業させることができます。プルリクエストが開かれる場合もあります。

これらのコマンドを使うと追加の worktree やリモート作業が作成されることがあるため、ワークショップでは任意としています。使う前に、範囲を明確にした新しいタスクを開始し、通常の手順で結果をレビューしてください。非同期エージェントについて詳しく学ぶ場合は、[cloud agent のワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、設定、セッションの共有、内部の動作確認ができます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使い、Copilot がコンテキストを管理する方法を確認しました。
- `/model` を使い、利用できるモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、クラウド エージェントへの委任に使える任意のコマンドを確認しました。

Copilot CLI にはほかにも多くのスラッシュ コマンドがあります。学んだことを[振り返り][next-lesson]、今後のステップを確認してワークショップを締めくくります。

## リソース

- [Copilot CLI のコマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
