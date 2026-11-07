---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスを扱い、Copilot が利用できるツールや自動化を追加する必要があります。MCP サーバーを使うと、Copilot CLI に組み込まれていないツールやサービスも利用できます。

このレッスンでは、次の操作を行います。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を理解します。
- まだ利用できない場合は Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルター機能を確認します。

## シナリオ

ユニットテストやエンドツーエンドテストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot が利用者のように開発中の Web サイトを使えるようにし、期待どおりに動作するという確信を高めます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは最新の情報にアクセスしたり、ユーザーに代わって操作を実行したりできます。

ツールやリソースへのアクセスは、AI エージェントと外部サービスをつなぐ MCP サーバーを介して行います。各 MCP サーバーは、エージェントが利用できる異なるツールやリソースを提供します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright によるブラウザー自動化を利用できます。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内の MCP を見つけやすくし、貢献できるように[MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーも、プロジェクトのほかの依存関係と同様に扱ってください。利用する前にソースコードと発行元を確認し、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot が利用者のように Web サイトを操作できるように追加します。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに設定されている場合は、このセットアップを省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認して <kbd>Enter</kbd> を押し、続けて <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を確認するよう Copilot に依頼する

これまでは、期待どおりに動くことを手動で確認しました。次は、Copilot で利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   アプリを起動し、Playwright MCP を使って、Issue と計画に照らしてフィルター機能を確認してください。変更は加えず、動作する点としない点を報告してください。完了したら、起動したサーバーを停止してください。
   ```

> [!NOTE]
> 通常は MCP サーバーを明示しなくても、Copilot が適切なサーバーを判断します。ここでは利用してほしいサーバーを指定することで、結果の一貫性を高め、トークンを節約できます。

2. Copilot がブラウザーで操作を行い、機能を確認する様子を見ます。
3. 報告を読み、すべてが期待どおりに動作するか確認します。

Copilot がサーバーを起動し、Playwright で Web サイトを操作して、サーバーを停止したうえで結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使って、実際のブラウザーで機能を確認しました。このレッスンでは、MCP と Copilot CLI での利用方法を学び、必要に応じてサーバーを追加し、エージェントにブラウザー操作を依頼しました。

次は、スキルとブラウザーツールをまとめた[QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
