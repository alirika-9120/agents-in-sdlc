---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローにおける特定のタスクや分野に合わせた、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダー内の Markdown ファイルでエージェントを定義し、Copilot が特定の種類の作業を効果的に行えるように、対象を絞った指示、ベスト プラクティス、コーディング パターン、分野固有の知識を提供します。チームの知識を再利用できるエージェントとしてまとめることができます。たとえば、[WCAG][wcag] への準拠を確認するアクセシビリティ エージェント、安全なコーディング プラクティスに従うセキュリティ エージェント、一貫したテスト パターンを保つテスト エージェントなどです。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダーにある Markdown ファイル、またはグローバルな `~/.copilot/agents` で定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown プロンプトを記述します。

### カスタムエージェントとエージェント スキルの違い

カスタムエージェントと[エージェント スキル][agent-skills-concept]には、役割が一部重複しています。どちらも主に Markdown ファイルで定義し、AI に作業の方法を指示します。最もわかりやすい違いは、**カスタムエージェントは作業者、スキルはツール**だということです。

カスタムエージェントには独自のコンテキスト ウィンドウがあり、作業の一環としてスキルやほかのエージェントを調整できます。このラボのアクセシビリティ カスタムエージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、プルリクエスト ワークフロー用やテスト実行・管理用のスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI のほかの使い方と同様に、環境やシナリオに合わせて試し、反復してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、誰もがクラウドファンディング プラットフォームを利用できるようにすることに取り組んでいます。最近、現在のダークテーマはテキストと背景色のコントラストが不十分で、読みにくいと感じる利用者がいることがわかりました。このアクセシビリティの課題に対応するため、デザイン チームは利用者が切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なので、この機能をできるだけ早く実装します。カスタムエージェントを使って機能を生成します。

この演習では、次の操作を行います。

- 既存のアクセシビリティ カスタムエージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装します。

## アクセシビリティ カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに作成されています。Copilot をどのようにガイドするか理解するため、内容を確認しましょう。

Codespace に戻り、ターミナルを開いて `main` からアクセシビリティ作業用の新しいブランチを作成します (演習 3 のフィルター PR とは分けておきます)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML フロントマターを確認します。

> [!CAUTION]
> `name` と `description` を含むフロントマターは、カスタムエージェントに必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成における主な責任
   - アクセシビリティのベスト プラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題や間違い

## Copilot Chat でカスタムエージェントを使う

VS Code では、`.github/agents` に定義したすべてのカスタムエージェントが、Copilot Chat ビュー下部のエージェント選択メニューに表示されます。エージェントを選択すると、そのエージェントの指示とツールを使うようにチャット セッションを設定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。演習ごとにモードとモデルの選択が異なります。必要な場合は、各手順で説明します。

1. Chat ビューのエージェント選択メニューで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部にあるエージェント選択メニューを選択します。既定では、現在のエージェントとして **default** が表示されています。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. アクセシビリティ エージェントに次のプロンプトを送信します。

   ```plaintext
   サイトにハイ コントラスト モードを追加してください。利用者が切り替えられるハイ コントラストのトグルを用意し、設定をブラウザーのローカル ストレージに保存して、ページを再読み込みしても保持されるようにしてください。
   ```

5. Copilot Chat がコードベースを調査し、変更を提案して、プロジェクト ファイルを編集します。編集内容は、ファイル パスとレビュー可能な差分とともにチャットに表示されます。
6. 編集が追加されると **Files changed** の表示が更新され、エージェントが変更したファイルを確認できます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot はリポジトリに実際の変更を加えます。Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集する場合があります。

次の演習では、この作業を監視して方向を調整します。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]について学びました。カスタムエージェントを使うと、チームの知識や標準を再利用できる形にまとめ、Copilot が特定のタスクや分野の作業を効果的に行えるようにできます。

この演習では、カスタムエージェントの定義方法と、Copilot Chat のエージェント モードでの使い方を学びました。

次は、[エージェントの作業を監視して方向を調整します][next-lesson]。作業中の変更をレビューし、同じセッションにライト モードのトグルを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
