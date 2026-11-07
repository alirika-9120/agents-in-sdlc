---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、任意のクラウドへの委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI にもさまざまなスラッシュコマンドがあります。高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次の操作を行います。

- `/context` と `/compact` で、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` で利用できるモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、クラウドエージェントへの委任に使う任意のコマンドを確認します。

## シナリオ

CLI の主要なワークフローが完了しました。次は、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルのコンテキスト ウィンドウの上限に近づくことがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュコマンドを使って自分で確認したり圧縮したりできます。

1. Codespace に戻り、まだ起動していない場合はリポジトリのルートから Copilot CLI を起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように分配されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を実行し、結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキスト ウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。自分でタイミングを選ぶ場合は `/compact` を使います。関連のない別タスクに移るときに新しい会話を始めるには、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、モデルの一覧を表示し、利用するモデルを選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況を確認します。
3. 現在のモデルを維持するか、別のモデルを選ぶか、<kbd>Esc</kbd> で一覧を閉じます。

## セッションを共有する

チームで協力し、学んだことを共有すると、AI ツールの活用を改善できます。`/share` コマンドでは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みバージョンで利用できる `/share` のオプションを確認します。
2. 共有する場合は、たとえば `/share file` でローカルの Markdown ファイルにエクスポートするなど、目的に合った方法を選びます。
3. エクスポートした内容を送信または公開する前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## 任意: 作業を拡張または委任する

ワークショップの主要な内容は完了しました。Copilot CLI には、大きなタスクに役立つコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割し、並行して実行できます。
- `/worktree` は、別タスク用の独立した Git worktree を作成できます。
- `/delegate` は、非同期で作業し、プルリクエストを開くこともある Copilot cloud agent にタスクを送信できます。

これらのコマンドは追加の worktree やリモート作業を発生させる可能性があるため、任意です。試すときは、範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業を詳しく学ぶ場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

スラッシュコマンドを使うと、Copilot CLI を構成したり、セッションを共有したり、内部の状況を確認したりできます。このレッスンでは、コンテキストの管理、モデルの確認、セッション共有、クラウドへの任意の委任を扱いました。

Copilot CLI にはほかにも多くのスラッシュコマンドがあります。次は、[学んだ内容を振り返り][next-lesson]、さらに学ぶための次のステップを確認します。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI でのコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
