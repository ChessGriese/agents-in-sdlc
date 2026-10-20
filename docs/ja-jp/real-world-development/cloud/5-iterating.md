---
title: "演習 5: GitHub Copilot の作業を反復する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントの管理][previous-lesson]

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスを改善し、機能を追加する複数のタスクを GitHub Copilot に依頼しました。コードにドキュメントを追加し、デザイン チームが改善するための関連ゲーム機能を作成し、ハイ コントラストとライト モードの切り替えなどアクセシビリティ機能を実装しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送り、成果を改善します。

### シナリオ

生成 AI を加えても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードを必ずレビューし、通常の DevOps プロセスに沿って作業します。チーム向けにコードレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について、GitHub Copilot の提案を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期で、監視されていない間に作業するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、作業用ブランチに対してのみ書き込みアクセス権を持ちます。
- Cloud agent は GitHub Actions 内で実行され、作業用の一時的な環境を作成します。
- GitHub Actions ワークフローの実行には、人間による承認が必要です。
- [外部リソースへのアクセスは既定で制限][agent-firewall]されています。MCP サーバーも制限の対象です。

## 生成されたドキュメントをレビューする

最初に、コードへドキュメントを追加するために GitHub Copilot cloud agent が作成したプルリクエスト (PR) を確認します。github.com の標準的な PR 画面を使用します。

> [!NOTE]
> PR に Copilot がファイアウォールでブロックされたという警告が表示されることがあります。既定では外部リソースへのアクセスが制限されているため、MCP サーバーへのアクセスを含め、これは想定された動作です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択してプルリクエストの一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** と表示されます。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあるので、休憩を取ったり、ここまで学んだことを振り返ったりしてください。

4. PR の準備ができたら、**Files changed** タブを選択して変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく更新されたコードを確認します。TSDoc の doc コメントなどが含まれます。正確な変更内容は異なります。

    変更を確認するときは、TSDoc の doc コメント、TypeScript の規約、ファイル ヘッダーに注目してください。これらは演習 2 の冒頭で確認したカスタム指示によるものです。

6. 更新内容を確認して問題がなければ、**Conversation** タブに戻って下へスクロールします。
7. ワークフローの実行に承認が必要であることを示す表示を確認します。
8. 承認が必要な場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが追加されます。すべて正常に進めば、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot と PR でやり取りするのは一方通行ではありません。チーム メンバーへのコメントと同じように、プルリクエストやコードのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、変更に対応するセッションを開始します。結果は確率的に変わるため、どのような依頼をすべきかを正確に指定することはできません。たとえば、次のような修正を依頼できます。

- 各コード ファイルの先頭に、そのファイルの役割を簡潔に説明するコメント ヘッダーを追加します。
- TypeScript と Astro のファイルに TSDoc の doc コメントを追加します。
- Astro アプリの構造を説明する README を作成します。

1. 上記の例などを参考に、コードベースのドキュメントに関する変更を **@copilot** にメンションして依頼します。
2. **View Session** を選択し、Copilot が作業する様子を確認します。更新のために新しいセッションが開始されます。
3. **Back to pull request** を選択して PR に戻ります。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

問題がなくなるまで反復できます。レビューの準備ができたら、下書き PR をレビュー可能な状態に変更して `main` ブランチにマージします。

![PR をレビュー可能な状態に変更](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue のために Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を開きます。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容を確認して問題がなければ、**Conversation** タブに戻って下へスクロールします。
6. ワークフローの実行に承認が必要であることを示す表示を確認します。
7. 承認が必要な場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションにワークフローが追加されます。すべて正常に進めば、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace を開き、ターミナルで次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えます。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。ほかの作業と同様に、Copilot cloud agent に更新を依頼して反復できます。たとえば、表示する関連ゲームの数を変更したり、コメント ヘッダーや TSDoc の doc コメントを追加したりできます。この Issue はカスタム指示の更新前に割り当てたため、Copilot が追加の指示を読み込んでいない点に注意してください。**Conversation** タブにコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントを使って実装した機能をレビューします。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 で mission control から追加を依頼したライト モードの両方が含まれるはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** と表示されます。Copilot が作業を完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注目します。
   - モードを切り替える UI コンポーネント
   - ユーザー設定の保存に使う local storage
   - ハイ コントラストとライト モードの CSS またはスタイルの変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モードの切り替えを管理する JavaScript/TypeScript コード

6. 更新内容を確認して問題がなければ、**Conversation** タブに戻って下へスクロールします。
7. ワークフローの実行に承認が必要であることを示す表示を確認します。
8. 承認が必要な場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが追加されます。すべて正常に進めば、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace を開き、ターミナルで次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名に置き換えます。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリを起動してブラウザーでハイ コントラストとライト モードの切り替えを試し、ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ カスタム エージェントが、Copilot にアクセシビリティのベスト プラクティスに沿って機能を実装するよう指示していることを確認しました。アクセシビリティに関する懸念や改善点があれば、前の PR と同じようにコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 委任を続ける

cloud agent に実際のバックログ項目を任せ、その間に別の作業に取り組む方法を身に付けましょう。リポジトリに Issue をいくつか追加して Copilot に割り当てます。例:

- ゲーム詳細ページに購入者向けの関心フォームを作成します。
- ゲーム一覧ページにページネーションを追加します。
- データアクセス ヘルパーに入力検証とエラー処理を追加します。

## まとめ

Cloud agent のハーネスを完了しました。これらのレッスンでは、次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し**、cloud agent の出力への影響を確かめました。
- **Copilot cloud agent に Issue を割り当て**、非同期に環境を準備し、計画して実行する様子を確認しました。
- アクセシビリティ向けの **カスタム エージェントを作成して使用し**、ハイ コントラストとライト モードの切り替えを追加しました。
- **Copilot agents ページを mission control として使い**、進行中のアクセシビリティ セッションを監視して案内しました。
- **cloud agent のプルリクエストをレビューして反復し**、`@copilot` に変更を依頼してワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent のハーネスを完了しました。他のハーネスでは、このワークショップで学んだ内容をさらに深められます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェント モードと MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェント スキル、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent に戻る `/delegate` を使い、ターミナルから変更を 2 つリリースします。

自分のリポジトリで、次のアイデアを試してみてください。

- リファクタリングやテスト カバレッジに関する Issue を cloud agent に割り当てます。
- 別の分野向けのカスタム エージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
