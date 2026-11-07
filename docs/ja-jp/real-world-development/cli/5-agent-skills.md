---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、報告形式をカスタマイズして、フィルター機能の検証に使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、コードが正しく動くか手動で検証したり、指示ファイルで標準に従うようにしたりできます。では、テストや lint、継続的インテグレーション (CI) のそのほかの作業はどうでしょうか。

このような作業には **エージェント スキル** が適しています。スキルを使うと、こうした処理を適切に実行する方法を Copilot に伝えられます。

このレッスンでは、次の操作を行います。

- 既存の `quality-checks` スキルを確認します。
- 実行結果の報告形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、ユニットテスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするため、報告形式を改善したいと考えています。

## 指示、スクリプト、リソース

エージェント スキルは、エージェントが必要に応じて読み込む、再利用可能なタスクの指示、実行可能なスクリプト、関連リソースをまとめたものです。基本的には、スキル名のフォルダーと、その中の Markdown ファイル `SKILL.md` で構成されます。Markdown には、スキル名と説明を定義するフロントマター、機能の概要、呼び出すタイミングの指針が含まれます。フォルダーに、スクリプトやほかのリソース用のサブフォルダーを追加することもできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトにあるコマンドを使うため、`SKILL.md` のみで構成されています。

スキルは、チームで共有して再利用するリポジトリ資産としてプロジェクトの `.github/skills` フォルダーに配置するか、ユーザーのスキル フォルダー `~/.copilot/skills` に配置できます。

## スキルを確認する

Tailspin Toys チームがユニットテスト、lint、型チェックを実行するために作成した `quality-checks` スキルを確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。説明によって、Copilot がスキルを呼び出すタイミングを判断しやすくなります。
3. 指示を読み、テストと lint の手順をどのように案内しているか確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更前にスキルを実行する

Copilot CLI では、スキルを直接呼び出すことも、自然言語で依頼することもできます。3つのチェックを実行するよう Copilot に依頼します。

1. Interactive モードのフィルター機能の会話に戻ります。
2. 次のプロンプトを入力します。

   ```plaintext
   quality-checks スキルを実行して、ユニットテスト、lint、型チェックを行ってください。
   ```

3. 最後に出力されるレポートを確認します。

## レポートをカスタマイズする

実行した内容、成否、ツールが報告した内容を確認できるレポートに改善します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. 次のセクションをファイルの末尾に追加します。

   ```markdown
   ## Results output formatting

   完了したら、実行した各コマンドと、成功、失敗、ブロックのいずれであったかを報告します。ツールが報告した場合に限り、テスト数、所要時間、エラー、警告、その他の指標を含めます。失敗やブロックがある場合は次に行う操作を示し、スキップまたは未完了のチェックを成功と表現しないでください。
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を加えたので、スキルの動作を確認します。Copilot CLI は、会話を再起動せずに編集済みのスキルを再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどと**まったく同じプロンプト**を使います。

   ```plaintext
   quality-checks スキルを実行して、ユニットテスト、lint、型チェックを行ってください。
   ```

3. レポートを読み、最初の結果と比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして使いました。このレッスンでは、ユニットテスト、lint、型チェック用の `quality-checks` スキルを確認し、結果の報告形式をカスタマイズして、再読み込み後に実行しました。

変更はフィルター機能の PR に含めます。次は、[Playwright MCP サーバーを使って Copilot がサイトを直接操作できるようにします][next-lesson]。

## スキルの例

次のコミュニティの例は参考資料であり、追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [貢献ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポート用スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
