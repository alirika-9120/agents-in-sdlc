---
title: "レッスン 8 - 機能 PR の作成とマージ"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストが、1つのブランチに保存されています。まとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** に手順を任せます。

> [!NOTE]
> 通常は機能、指示、スキル、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡潔にするため、フィルター機能と品質に関する作業を1つの会話とブランチにまとめ、この PR に含めています。

このレッスンでは、次の操作を行います。

- Agent Merge と、マージまでの作業を自動化する方法を学びます。
- 機能全体の変更と検証結果を確認します。
- フィルター機能の PR を作成します。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能の作業を通じて、Copilot で機能を計画、実装、検証しました。Tailspin Toys では、マージの許可は開発者が管理しながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題 (失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要に応じたリベースなど) に取り組みます。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは **Merge pull request** を自分で選択しました。Agent Merge にその作業を任せることもできます。有効にする前に、作業をレビューし、マージ可能な状態か判断してください。

## Agent Merge で PR を管理する

コードがそろったら、全体をレビューして PR を作成し、残りの手順を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを確認します。すべて成功すると、マージが実行されます。
7. 「PR #14 was squash-merged successfully.」のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。ブロックされた場合は報告された理由を確認し、リポジトリで許可されていれば、レビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの手順など、開発プロセスの一部を自動化しました。このレッスンでは、Agent Merge を学び、機能全体と検証結果を確認し、フィルター PR を作成して、レビュー後にマージを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に役立つ[その他の Copilot CLI スラッシュコマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
