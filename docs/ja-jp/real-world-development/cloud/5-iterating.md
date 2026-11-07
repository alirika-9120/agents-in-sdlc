---
title: "演習 5 - GitHub Copilot の作業を反復する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善や機能追加を目的とする複数のタスクで GitHub Copilot を使いました。コードへのドキュメント追加、デザイン チームが反復できる関連ゲーム機能の作成、ハイ コントラストとライト モードの切り替えなどのアクセシビリティ機能を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って作業を改善しましょう。

### シナリオ

繰り返し説明してきたとおり、生成 AI を導入してもソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全体のレビューを有効にする前に、GitHub Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能の提案を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、監視を受けずに作業するため、安全性を保つためのセキュリティ制約が設けられています。

- Copilot はリポジトリへの読み取りアクセスを持ち、書き込みアクセスはコード変更用のブランチに限定されます。
- cloud agent は GitHub Actions 内で実行され、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローの実行には、人間による承認が必要です。
- [外部リソースへのアクセスは既定で制限されます](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーも対象です。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認するときに、ファイアウォールによって GitHub Copilot がブロックされているという警告が表示される場合があります。Copilot は MCP サーバーなどの外部リソースへのアクセスが既定で制限されているため、これは**想定される動作**です。必要であれば、[Copilot cloud agent のファイアウォールを構成または無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだタスクを実行中の場合、PR に **[WIP]** が表示されます。その場合は、Copilot が作業を完了するまで待ちます。数分かかることもあるので、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選び、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc のドキュメント コメントなど、更新されたコードを確認します。実際の変更内容は状況によって異なります。

   TSDoc コメント、TypeScript の規則、コメント ヘッダーを確認します。これらは演習 2 の最初に確認したカスタム指示ファイルによるものです。

6. 更新をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示すインジケーターがあるか確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローがキューに登録されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot と PR 上でやり取りする方法は、作業を一方的に受け取るだけではありません。チーム メンバーに依頼するのと同じように、PR やコードへのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、新しいセッションで変更に対応します。結果は確定的ではないため、必ず同じ結果になるプロンプトは示せません。たとえば、次のような変更を依頼できます。

- 各コード ファイルの先頭に、ファイルの役割を短く説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメント コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例などを参考に、ドキュメントに関する変更をコメントで依頼し、他のユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると PR に戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選び、変更をレビューします。

満足するまで反復できます。問題がなければ、PR をドラフトからレビュー可能な状態に変更して `main` ブランチにマージします。

![PR をレビュー可能な状態に変更する画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示すインジケーターがあるか確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションにワークフローがキューに登録されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストすることもできます。Codespace に移動してターミナルを開き、`<branch-name>` を Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を実装しました。ほかの作業と同じように、Copilot cloud agent に追加の変更を依頼できます。たとえば、表示する関連ゲームの数を調整したり、コメント ヘッダーや TSDoc コメントを追加したりできます。後者を依頼する場合は、指示を更新する前に割り当てたタスクであることに注意してください。前の PR と同様に、**Conversation** タブにコメントを追加すると、Copilot が内容を確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタムエージェントを使って実装した機能を確認します。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 の Mission control から追加を依頼したライト モードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。数分かかることがあるので、完了するまで待ちます。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点を確認します。
   - モードを切り替える UI コンポーネント
   - 利用者の設定を保持するローカル ストレージの使用方法
   - ハイ コントラスト モードとライト モードの CSS またはスタイルの変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 更新をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示すインジケーターがあるか確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローがキューに登録されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、`<branch-name>` を Copilot が作成したブランチ名に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイ コントラストとライト モードのトグルをテストします。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ カスタムエージェントが、アクセシビリティのベスト プラクティスに沿って機能を実装するよう Copilot をガイドしたことがわかります。アクセシビリティ上の問題や改善点があれば、前の PR と同じようにコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 委任を続ける

cloud agent は、実際のバックログ項目を渡して別の作業に取り組めるときに効果を発揮します。習慣を身につけるために、リポジトリに Issue をいくつか追加して Copilot に割り当ててみましょう。たとえば次のような内容があります。

- ゲーム詳細ページに支援者向け問い合わせフォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。これらのレッスンでは、次の操作を行いました。

- リポジトリに含まれるカスタム指示ファイルを確認し、後で cloud agent の出力に反映される様子を確かめました。
- Copilot cloud agent に Issue を割り当て、環境のセットアップ、計画、非同期の実行を確認しました。
- アクセシビリティ用のカスタムエージェントを作成して使い、ハイ コントラストとライト モードのトグルを追加しました。
- Copilot agents ページを Mission control として使い、作業中のアクセシビリティ セッションを監視して方向を調整しました。
- cloud agent のプルリクエストをレビューして反復し、`@copilot` に変更を依頼して、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。さらに学ぶ場合は、ここで学んだ内容を別のハーネスで試してください。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェントモードと MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェント スキル、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使い、ターミナルからレビュー済みの変更を2つリリースします。

自分のリポジトリでは、次のアイデアを試してください。

- リファクタリングやテスト範囲の Issue を cloud agent に割り当てる。
- 別の分野向けのカスタムエージェントを作成する。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
