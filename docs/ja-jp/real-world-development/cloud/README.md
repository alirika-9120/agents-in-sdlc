---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウドで非同期に作業できます。GitHub でタスクを割り当てると、cloud agent がバックグラウンドでリポジトリを調査し、変更を行い、プルリクエストを作成します。その間、ほかの作業を続けられます。

この演習では、まず cloud agent が従うカスタム指示を追加し、GitHub Issue を割り当てて実装を依頼します。カスタムエージェントを確認して使用し、エージェントのダッシュボードからセッションを監視して方向を調整し、最後にプルリクエストをレビューして結果を反復します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. カスタム指示][ex1] | コンテキスト | cloud agent が従うカスタム指示を追加します |
| [2. Cloud Agent][ex2] | 非同期エージェント | GitHub Issue を Copilot cloud agent に割り当てます |
| [3. カスタムエージェント][ex3] | 専門エージェント | カスタムエージェントを確認して使用します |
| [4. エージェントの管理][ex4] | 監視 | エージェントのセッションを監視し、方向を調整します |
| [5. 反復][ex5] | レビュー | PR をレビューし、Copilot の作業を反復して、次の手順を選びます |

## 前提条件

このワークショップに参加する前に、次の項目を確認してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランを利用できる GitHub アカウント
- [ ] アカウントまたは組織で有効になっている Copilot cloud agent

> [!TIP]
> 有料プランを利用していませんか？認証済みの学生は、[GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI が含まれているため、すべてのハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> cloud agent は Copilot Free を除く、**Copilot Student、Pro、Pro+、Business、Enterprise** のすべてのプランで利用できます。Copilot Business または Enterprise では、管理者が組織で有効にする必要があります。

> [!NOTE]
> このハーネスでは MCP を扱いません。Copilot で MCP サーバーを使う方法を学ぶには、[CLI ハーネス](../cli/)または [VS Code ハーネス](../vscode/)を確認してください。

## はじめに

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
