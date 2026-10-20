---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用できるカスタマイズ、参考資料を振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連の作業に取り組みました。具体的には、次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、フィルタリング Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果をレビューして、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリング Issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの検証を確認しました。
- フィルタリングの変更全体をレビューし、フィルタリング PR に Agent Merge を承認しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュ コマンドを確認しました。

## リリースした内容

このワークショップでは、更新した `main` から作成した別々のブランチで、2 つの PR を作成します。

1. **星評価**: 既存の `starRating` と、未評価の場合の状態をゲームカードに表示します。
2. **フィルタリングと品質ワークフロー**: フィルタリングを実装し、指示を更新して機能に適用します。また、`quality-checks` レポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルタリングの計画から PR の作成まで、同じ会話とブランチを使用しました。ワークショップを簡潔にするため、作業を 1 つの PR にまとめました。

## 複数の方法で検証する

自動テスト、自分で行うブラウザー確認、MCP を使った Copilot のブラウザー操作でコードを確認しました。`quality-checks` スキルはユニット テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA では、それらの結果と要件およびテスト範囲の確認をまとめてから PR に進みました。

追加するテストは、実際に不足しているカバレッジを補う必要があります。QA を実行して新しいテストが不要と判断されることもあります。マージを承認する前に、コードと根拠を確認し、変更後は影響を受ける検証を再実行してください。

## ベストプラクティス

Copilot に提供するコンテキストとツールが、作業結果に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整できます。指示は標準を定め、スキルは繰り返し可能なタスクを記述し、カスタム エージェントは専門の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。構築前の検討には **Plan**、対象を絞った変更で対話を続けるには **Interactive**、範囲を明確にしたタスクには **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業には高機能なモデルを選びます。

インフラストラクチャと同様に、コンテキストも重要です。*何を*構築するか、*なぜ*必要か、*どのように*実装するかを明確に説明すると、結果が大きく変わります。

## さらに確認する

主要なワークフローを学びました。Copilot CLI の次の機能も試してみてください。

- `/review` でコードレビュー エージェントに変更の分析を依頼します。
- `/rubber-duck` で問題を説明し、別の視点から考えます。
- `/fleet` で独立したサブタスクを並列に調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送ります。

## 次のステップ

ツールを上達させるには、使い続けることが一番です。本番コードや個人のコード、長年作りたいと思っていた小さなアプリなどで試してください。学んだことをチームと共有し、ほかの人の知識も学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境を比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI のコマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
