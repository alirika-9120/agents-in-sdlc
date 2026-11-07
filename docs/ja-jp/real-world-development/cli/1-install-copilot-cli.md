---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、基本操作を確認して、事前に用意されたフィルター機能の Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディングアシスタントです。コードベースの探索、コード生成、コマンド実行、外部ツールとの連携をすべてコマンドラインから行えます。タスクを任せたり、変更を依頼したりしながら、作業に集中できます。最初のステップは、ツールのインストールです。幸い、使い慣れたツールでインストールできます。

このレッスンでは、次の操作を行います。

- npm を使って GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP サーバーを使って、フィルター機能の Issue を見つけます。

## シナリオ

チームでは、増え続けるバックログに対応するため、AI エージェントを使い始めています。Copilot CLI は、多くの開発者が日常的に使うターミナルで、その機能を利用できるようにします。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで使える状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js がプリインストールされているため、npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされており、バージョン要件を満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められた場合は、デバイスコードの手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されているパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して信頼します。

> [!NOTE]
> Codespace では、GitHub セッションを通じて既に認証されている場合があります。認証を求められずに Copilot CLI が起動した場合は、そのまま続行できます。

## 基本操作を確認する

通常のシェルプロンプトで入力したコマンドは、Codespace で直接実行されます。Copilot CLI を起動すると、自然言語の入力はエージェントに送信され、スラッシュコマンドは会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストールしたバージョンで利用できるコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. Copilot に簡単な質問をして、動作を確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 回答を読み、Copilot が答える前にリポジトリを調べる様子を確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用できることを確認します。
6. Copilot にフィルター機能の Issue を探してもらいます。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて Issue を読みます。最初の小さな変更を終えた後、この Issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチを使用し、worktree を自動作成しません。各変更の前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は通常、設定済みのアクセス許可の範囲外でツールを使う前に確認します。このワークショップでは、`--yolo` を使って再起動します。これにより、Codespace 内でアクセス許可の確認がなくなり、演習の内容に集中できます。

> [!CAUTION]
> `--yolo` は、すべての自動アクセス許可 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を有効にします。Codespace や VM などの隔離された環境でのみ使用し、日常の開発用の既定コマンドとしてエイリアスに設定しないでください。詳細は[ツールの使用を許可または拒否する][allow-all-warning]を参照してください。

`--enable-all-github-mcp-tools` を指定すると、後続のレッスンで Issue や pull request の操作に使う、読み取りと書き込みの GitHub MCP ツールが有効になります。Codespace からローカルコンピューターにはアクセスできませんが、認証済みの GitHub リソースには引き続きアクセスできます。公開やマージの前に変更をレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから、次のコマンドで再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて簡単な質問をし、会話が動作することを確認してから `/exit` で終了します。

Copilot は会話を自動的に保存します。後のレッスンでは、指示を変更したりエージェントを追加したりした後に `copilot --resume` を使って、同じ機能開発の会話とブランチに戻ります。

## まとめと次のステップ

GitHub Copilot CLI のインストールと認証が完了しました。このレッスンでは、次の操作を行いました。

- npm を使って GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーを使って、フィルター機能の Issue を見つけました。

次は、最初の変更に取りかかり、ゲームカードに星評価を表示するよう Copilot CLI に依頼します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
