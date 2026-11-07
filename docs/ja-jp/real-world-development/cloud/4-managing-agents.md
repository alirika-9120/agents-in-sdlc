---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

これまでの演習では、ユーザー エクスペリエンスの改善や機能追加を目的とする3つのタスクを GitHub Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクは、GitHub.com の[エージェント ページ][agents-page]など、さまざまなツールで管理できます。この Mission control では、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。実行内容を調べたり、進行中のセッションに追加の指示を送ったりできます。

このレッスンでは、次の操作を行います。

- エージェント ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

ハイ コントラスト モードの作成をエージェントに割り当てた後、チームはライト モードも追加するよい機会だと考えました。サイトのスタイル更新と切り替え機能をすでに実装しているため、ライト モードも含めるのが自然です。エージェントの作業に指示を追加し、ハイ コントラストに加えてライト モードも実装してもらいます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインのタスク一覧を確認します。次のようなタスクが表示されます。
   - コードのドキュメントを更新する
   - ゲームを変更する API を生成する
   - Web サイトにハイ コントラスト モードを追加する
3. 実行中のタスクを選択し、Copilot が行った操作を確認します。たとえば、次のような操作があります。
   - リポジトリからコードをチェックアウトする
   - Copilot の作業環境を作成する
   - MCP サーバーをセットアップする
   - 割り当てられたタスクを完了するために、さまざまな作業を行う

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。追加の監視に使える PR と変更ファイルが表示されます。

## cloud agent に追加の指示を送る

実行中のタスクを確認したので、ハイ コントラスト モードを作成中の Copilot に、ライト モードのトグルも追加するよう依頼します。

1. ハイ コントラスト モードの作成を示すセッションを選択します。正確なタイトルは Copilot が付けた名前や作業状況によって異なります。

    ![Mission control に表示されるアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を開始したことがわかるまで数分待ちます。次のようなメッセージが表示されたら作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    ハイ コントラスト モードに加えて、ライト モードも追加してください。このモードにも切り替えスイッチを用意し、利用者が希望する表示モードを選べるようにしてください。
    ```

    ![エージェント ページで、作業中の Copilot セッションに追加の指示を送るダイアログが強調表示されたスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを認識し、作業に取り込むことを確認します。

## Copilot に作業を続けてもらう

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行中の手順が終わると、新しい依頼が作業に加わります。

今回も完了まで数分かかります。その間に、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot agents ページは、GitHub Copilot cloud agent のタスクを監視し、指示を出すための中心的な場所です。Mission control で実行中と完了済みのタスクを追跡し、作業内容を確認し、進行中のセッションに追加の指示を送る方法を学びました。

このレッスンでは、Mission control とエージェント ページでタスクを監視し、進行中のセッションに機能追加の指示を送りました。

Copilot がアクセシビリティ機能を完成させている間に、次は[作成されたプルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
