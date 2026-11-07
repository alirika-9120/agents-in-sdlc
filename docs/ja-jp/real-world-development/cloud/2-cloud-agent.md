---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

多くの組織では、解決されていないセキュリティの問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能リクエストなど、技術的負債に直面しています。GitHub Copilot の cloud agent は、コードの更新や機能の追加などを自律的に行います。エージェントの作業が終わると、人間がレビューできるドラフト PR が作成されます。面倒な作業を委任して開発を加速し、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを行います。

- コード生成環境をカスタマイズします。
- 操作が安全に行われることを確認します。
- Issue の範囲を明確にする重要性を学びます。
- Issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には解消したい技術的負債があります。最初のサイトを開発した委託先がドキュメントを適切に整備しませんでした。実際、ドキュメントはまったくありません。最初の作業として、アプリケーションのエクスポート関数すべてに TSDoc のドキュメント コメントを追加したいと考えています。

さらに、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリのほかのタイトルを「関連ゲーム」として表示し、利用者が閲覧を続けられるようにします。まだ完成度の高い実装は必要なく、UX の受け入れテストに使えるものを求めています。この作業は現在ブロッカーですが、より優先度の高い Issue がほかにあります。

どちらも、優先度を下げられてしまいやすいタスクです。Copilot cloud agent に割り当てると、非同期に作業を進めてもらえます。その間、別の作業に集中し、後から Copilot の成果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。たとえば、[GitHub Issue を Copilot に割り当て][assign-issue]ます。割り当てると、Copilot は進捗を追跡するドラフトのプルリクエストを作成し、環境をセットアップして、タスクに取り組み始めます。作業中または完了後に Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 適切な範囲の指示が重要

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく見ると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲を適切に定めてください][cloud-agent-best-practices]。AI ペア プログラマーとして扱い、段階的に作業し、学び、試し、必要に応じて調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot のガイド方法を確認しました。カスタム指示の演習をすでに行った場合は、`.github/copilot-instructions.md` の効果を確認しています。まだの場合は、cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認しておきましょう。

リポジトリの GitHub Web UI、または実行中の Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の規則と TSDoc ドキュメント コメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` フロントマターが `**/*.test.ts` ファイルに指示を適用していることを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てる際、生成された PR に TSDoc コメント、TypeScript の規則、コメント ヘッダーが反映されているか確認してください。後の演習で PR をレビューするときにも、改めてこの点を確認します。

## Copilot cloud agent の開発環境をセットアップする

コードの作成では、関係者にかかわらず、通常は特定の環境やセットアップ スクリプトが必要です。Copilot にタスクを割り当てる場合も同様で、Copilot はソフトウェア エンジニアのように作業します。

cloud agent は、作業時の環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップ ワークフロー][setup-workflow]を作成し、作業開始前に実行できます。これにより、必要な開発ツールや依存関係を用意できます。このラボでは、学習を円滑に進められるよう、事前に設定されています。単一の Astro アプリに必要な Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションおよびシード済みの SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 必要な手順に対して、可能な限り最小限の権限を設定します。Copilot の操作には専用のトークンが使われます。
    permissions:
      # セットアップ手順でリポジトリをクローンする場合 (依存関係のインストールなど) は、contents: read 権限が必要です。クローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Astro と Drizzle/libSQL を使うアプリの Node.js セットアップ
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテスト用のローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じ形式ですが、いくつか重要な点があります。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは、Copilot が PR 作業を開始する前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot の作業開始を待つ代わりに、ワークフローが正常に実行されるかテストできます。

## ドキュメントを追加する

ドキュメントが重要だと誰もが理解していても、多くのプロジェクトには古い情報しかないか、ドキュメントがまったくありません。このような技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成を得意とするため、cloud agent に割り当てるのに適した Issue です。Copilot はバックグラウンドで必要なドキュメントを作成します。後の演習で成果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   関数やメソッドには、役立つ場合に TSDoc のドキュメント コメントを含めることが、組織の要件です。しかし、最近の更新ではこの標準が守られていません。動作を明確にするうえで有用なドキュメント コメントが含まれるよう、既存のコードを更新する必要があります。
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

    ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。一覧に新しい PR が表示されない場合は、しばらく待ってからブラウザーを更新します。
12. 数分後、Copilot が TODO リストを作成していることを確認します。

> [!NOTE]
> Copilot の TODO リストが PR に表示されるまで数分かかる場合があります。前述のワークフローで環境を作成し、プロジェクトを分析して、最適な取り組み方を判断しているためです。

13. リストと、Copilot がこれから行うタスクを確認します。
14. プルリクエストのタイムラインを下へスクロールし、Copilot が Issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot のセッション画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新されたインジケーターを表示するために、ウィンドウの再読み込みが必要な場合があります。

16. 実行中のセッションをスクロールし、Copilot がどのように問題を解決しているか確認します。コードを調査して状態を理解し、方針を検討してからコードを作成する様子を見ることができます。

完了まで数分かかる場合があります。Copilot cloud agent の主な目的の1つは、タスクを非同期に実行し、ほかの作業に集中できるようにすることです。この機能を活用し、Copilot cloud agent にもう1つタスクを割り当ててから、アプリケーションへの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点は、作業を分けられることです。デザイン チームが求める関連ゲームの表示は、それほど時間がかからないとしても、その時間をほかの作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

    ```markdown
    ゲーム詳細ページに関連ゲームを表示して、利用者がほかのゲームを見つけやすくします。デザイン チームは UX を検討し、受け入れテストを行いたいと考えています。要件は次のとおりです。

   - `src/lib/` に、指定されたゲームと同じカテゴリに属するほかのゲームを返すデータ アクセス ヘルパーを追加します。指定されたゲーム自身は結果から除外します。
   - ゲーム詳細ページにヘルパーを使った「Related games」セクションを表示します。
   - 関連ゲームがない場合にも対応します。
   - 新しいヘルパーのユニットテストを作成します。
   - PR を作成する前に、すべてのテストが成功することを確認します。
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

    ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

その後、Issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことがわかります。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェア エンジニアのように作業するので、常に監視する必要はありません。完了後にレビューします。次は、カスタムエージェントの作成と使用を確認しましょう。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使って、Issue を Copilot に非同期で割り当てる方法を学びました。cloud agent は、技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などに使えます。

この演習では、次の操作を行いました。

- コード生成環境をカスタマイズしました。
- 操作を安全に行う方法を確認しました。
- Issue の範囲を明確にする重要性を学びました。
- Issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントを作成して使用します。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
