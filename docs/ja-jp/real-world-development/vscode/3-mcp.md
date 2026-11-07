---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェント モードでフィルター機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認しましょう。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次の操作を行います。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にブラウザー上でフィルター機能を手動テストするよう依頼します。
- ブランチを公開し、フィルター機能のプルリクエストを開きます。

## Model Context Protocol (MCP) とは

エージェント モードは、エディターの外部にアクセスできると、さらに多くの作業を行えます。Copilot がそれを可能にする方法が Model Context Protocol (MCP) です。エージェントが外部のツールやサービスと通信するための標準です。

![エージェント モードの内部動作と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP により、リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作を行ったりできます。

ツールやリソースへのアクセスは、AI エージェントと外部サービスをつなぐ MCP サーバーを介して行います。MCP サーバーは、既存の API や npm パッケージなどのローカル ツールとの通信を管理します。各 MCP サーバーは、エージェントが利用できる異なるツールやリソースを提供します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API を利用できます。新しいリポジトリの作成、既存リポジトリの更新、Issue やプルリクエストの管理などをエージェントに依頼できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化を利用できます。Web ページへの移動、フォームへの入力、ボタンの選択などをエージェントに依頼できます。

ほかにも多くの MCP サーバーがあり、さまざまなツールやリソースを利用できます。GitHub は、エコシステム内の MCP を見つけやすくし、貢献できるように[MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーもプロジェクト内のほかの依存関係と同様に扱ってください。利用する前にソースコードと発行元をよく確認し、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密情報や重要な操作へのアクセスを許可するときは注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` の設定はプロジェクト テンプレートに含まれています。そのため、前の演習では Copilot がバックログの Issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub の認証を行います。

## Playwright MCP サーバーを追加する

次は2つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot がブラウザーを操作できるので、機能のテストに役立ちます。

1. `.vscode/mcp.json` の `github` と同じ階層に `playwright` の設定を加え、ファイルが次のようになるようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモート サーバーの `github` とは異なり、`playwright` は**ローカル** サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは、画面を表示せずにブラウザーを実行する指定です。デスクトップ環境がない Codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys ではエンドツーエンド テストにすでに Playwright を使っているため、必要なブラウザーもインストール済みの場合がほとんどです。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行してから再試行するよう依頼します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot がサーバーのツールを初めて使うときに VS Code が起動し、サーバーを信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` の上に **Start** 操作が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code がサーバーを信頼するか確認したら、構成を確認して信頼することを選びます。これによりサーバーが起動できます。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot が初めてツールを使うときに VS Code がサーバーを起動し、信頼を確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作して変更を確認する代わりに、エージェントがページを開き、移動し、フィルターを適用して、その結果を確認して報告します。会話を続けたまま、機能が期待どおりに動作するかすばやく確かめられます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。ボタン、リンク、リスト項目など、ラベル付きの構造化された要素を基にエージェントが判断します。支援技術と同じように情報を扱うため、簡単な機能テストがアクセシビリティの基本確認にもなります。

サーバーに接続してアプリを実行した状態で、Copilot にフィルター機能を確認してもらいます。

```text
Playwright MCP サーバーを使って、実行中の http://localhost:4321 にブラウザーでアクセスし、新しいゲームのフィルター機能を確認してください。

1. ゲームの一覧ページを開き、ゲームがいくつ表示されているか記録します。
2. カテゴリで絞り込み、選択したカテゴリのゲームだけが表示されることを確認します。
3. カテゴリの絞り込みを解除し、パブリッシャーで絞り込んで、そのパブリッシャーのゲームだけが表示されることを確認します。
4. カテゴリとパブリッシャーを組み合わせ、両方の条件が結果に反映されることを確認します。

各手順で確認した内容を報告し、期待どおりに動作しない点があれば指摘してください。
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。Issue の受け入れ基準に照らして報告を読みます。問題があれば追加の質問をしたり、プルリクエストを作成する前にコード修正を依頼したりします。

> [!NOTE]
> テストするには、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Copilot が Playwright MCP サーバーを初めて使用するときに、ブラウザーをダウンロードする場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行してから再試行するよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したので、チームのレビュー用にプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace で **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![Copilot が行った変更を示す Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

プルリクエストを作成する方法は、github.com や GitHub コマンド ライン インターフェイス (CLI) など複数あります。今回は GitHub Copilot を使っているので、PR の作成を依頼しましょう。Copilot は関連する Issue を見つけ、その Issue に関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェント選択メニューで **Agent** を選択し、Copilot が GitHub ツールを使えるようにします。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   リポジトリでカテゴリとパブリッシャーによる絞り込みに関する Issue を探してください。現在のブランチの新しいプルリクエストを作成し、該当する Issue に関連付けてください。
   ```

4. 情報の取得や操作を行うため、必要に応じて **Continue** を選択して Copilot に許可を与えます。GitHub ツールを初めて使うときは、GitHub への認証を求められる場合があります。プロンプトに従って認証します。
5. Copilot が Issue を検索し、適切なものを見つけて PR を作成することを確認します。
6. Copilot が生成したリンクを選択して PR をレビューします。ただし、まだ**マージしないでください**。

## まとめと次のステップ

この演習では、次の操作を行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- リリース前に、ブラウザーでフィルター機能を手動テストしました。
- ブランチを公開し、フィルター機能のプルリクエストを開きました。

MCP サーバーを使って機能を**テスト**しましたが、MCP は Copilot を拡張する方法の1つにすぎません。次は、アクセシビリティのレビューなど、対象を絞ったタスクを効率化する[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
