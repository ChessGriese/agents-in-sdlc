---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメントの標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときはコンテキストが重要です。特定の方法でタスクを実行してほしい場合は、その指針を Copilot が参照できるようにします。[指示ファイル][instruction-files]には、求めるコードだけでなく、コードの構成方法も記載します。フィルタリング機能を実装したので、Copilot が参照した指示を確認し、ドキュメントの標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパス別の指示がエージェントに届く仕組みを確認します。
- コーディング標準に従うよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発作業のガイドラインと要件があります。たとえば、次のようなものです。

- コメントではコードを言い換えず、意図や自明ではない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数では、目的、パラメーター、戻り値を TSDoc/JSDoc で文書化します。該当する場合は、注入可能な `db` 引数も記載します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を文書化し、関連コードを変更したときにコメントも最新の状態に保ちます。
- 既存の書式設定と lint の指針を維持します。

指示ファイルを使って、Copilot がこれらの慣行に沿って作業できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディング スタイルや要件を Copilot に伝え、より適切な提案やコードを生成できるようになります。推奨する規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定のファイルやタスクを対象に指示を作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリに対する**すべての**リクエストで Copilot に送られる単一の指示ファイルです。技術スタック、構築するものの概要、全般的なベストプラクティスなど、多くのチャットや CLI リクエストに関係するプロジェクト情報を記載します。
- `.github/instructions/*.instructions.md`: 特定の言語、ファイルの種類、タスク向けのガイドラインを記載します。

> [!NOTE]
> その他の指示形式の対応状況は環境によって異なります。形式を利用する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

作業を始めるため、スターター プロジェクトに含まれる指示ファイルを確認し、変更前の状態を把握します。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく)、`.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。この指示は、このリポジトリにおける Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro、Drizzle データレイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob は、指示が適用されるファイルを指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存ファイルへのリンクを確認します。これらを使って、大きな指示セットを再利用しやすいファイルに分け、参考例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、追加すべき内容があります。`copilot-instructions.md` を変更し、新しく生成する TypeScript に TSDoc コメントを付けるよう指示します。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新した指示を適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存したら、会話を終了して再開し、機能のコンテキストを保ったまま新しい指示を読み込ませます。

1. フィルタリングの会話を続けている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   選択肢が表示されたらフィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. Copilot に更新した指示を適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して変更された TypeScript ファイルを確認します。新しい TSDoc コメントが正確にコードを説明していることを確認します。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを取得する仕組みを確認し、機能に新しい標準を適用しました。具体的には、次のことを行いました。

- リポジトリの指示とパス別の指示がエージェントに届く仕組みを確認しました。
- コーディング標準に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
