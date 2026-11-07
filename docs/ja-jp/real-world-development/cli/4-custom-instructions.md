---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使ううえで、コンテキストは重要です。タスクを特定の方法で進める必要がある場合、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]では、求めるコードだけでなく、その構成方法も説明できます。フィルター機能を実装したので、Copilot が参照した指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの指示とパス指定の指示ファイルがエージェントに適用される方法を確認します。
- コーディング標準に従うよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントではコードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数では、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。該当する場合は、注入可能な `db` 引数も含めます。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連コードが変わったらコメントも最新に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらのプラクティスに沿ってタスクを実行できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードを生成できるようにします。コーディング規則、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定の種類のファイルに対して指示を作成できます。

指示ファイルには2種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送られる単一の指示ファイルです。多くのリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスク向けのガイドラインを記載します。

> [!NOTE]
> ほかの指示形式やサポート状況は、環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、作業を始めるための指示ファイルが含まれています。変更を加える前に、既存の指示を確認して効果を把握しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。この指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro、Drizzle データレイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを決める glob を指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存のプロジェクトファイルへのリンクを確認します。指示を再利用できる小さなファイルに分け、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、生成される TypeScript に TSDoc コメントを追加するよう、コアの `copilot-instructions.md` にルールを追加します。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** という見出しのセクションを探します。35行目付近にあります。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - 新しい TypeScript には、ドキュメントとして TSDoc コメントを記述してください。
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使用する

Copilot CLI は、会話の開始時にリポジトリの指示を読み込みます。指示を更新したら、フィルター機能の会話を終了して再開します。これにより、機能のコンテキストを保ったまま、新しいガイダンスを読み込めます。

1. フィルター機能の会話が開いている場合は、`/exit` を入力します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートで、`game-filters-cli` ブランチから保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   会話の選択を求められた場合は、フィルター機能の会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   指示とコードのガイダンスを更新しました。生成したコードをこのガイダンスに合わせて更新してください。
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、その説明がコードの内容と一致することを確かめます。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを得る方法を確認し、新しい標準を機能に適用しました。このレッスンでは、次の操作を行いました。

- リポジトリの指示とパス指定の指示ファイルがエージェントに適用される方法を確認しました。
- 指示ファイルを更新し、新しい標準を追加しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
