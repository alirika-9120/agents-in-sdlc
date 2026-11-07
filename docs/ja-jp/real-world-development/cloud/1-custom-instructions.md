---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使ううえで、コンテキストは重要です。タスクを特定の方法で行う必要がある場合や、Copilot に背景情報を伝える必要がある場合は、そのコンテキストを利用できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」行うかだけでなく、「どのように」行うかも伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパス指定の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここでは変更前後のプロンプトを実行しません。Copilot cloud agent は GitHub Issue に対して非同期に動作するため、リアルタイムで並べて効果を示すのが難しいためです。このハーネスの後半で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データレイヤーには、必ずユニットテストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- TSDoc のドキュメント コメントを使って、コードにドキュメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、Copilot がこれらのプラクティスに沿ってタスクを行えるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードを生成できるようにします。好みのコーディング規則やライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定の種類のファイルに対して指示を作成できます。

指示ファイルには2種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送られる単一の指示ファイルです。Copilot へのチャットや CLI の多くのリクエストに関係する、プロジェクト全体の情報を記載します。技術スタック、開発対象の概要、ベスト プラクティス、全般的なガイダンスなどが該当します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に対して作成できます。TypeScript や Astro などの言語、UI コンポーネントの作成やユニットテストの追加などのタスク向けにガイドラインを記載します。

> [!NOTE]
> IDE で作業する場合、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの指示と、`applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は、AGENTS.md や CLAUDE.md など、ほかの標準に基づく指示ファイルも[サポートしています][custom-instructions-support]。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成に関する詳しい説明は、このワークショップの範囲外です。ただし、サンプル プロジェクトには代表的な構成例があります。概要は次のとおりです。

- `copilot-instructions.md` には、開発対象の説明、プロジェクトの構造、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種別や、特定のタスクに関する指示を記載します。
- 自然な言葉を使い、明確なガイダンスを記載します。コードをどのように書くべきか、また書くべきでないかの例を示します。

指示ファイルの作成方法は1つではありません。AI の使い方と同様に、プロジェクトに最適な方法は試しながら見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、指示ファイルを充実させることをお勧めします。このプロジェクトのファイルを確認すると、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの指示があります。
>
> Copilot に指示ファイルを生成してもらうこともできます。方法は利用する環境によって異なります (たとえば、VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。このワークショップでは、該当する手順で各環境の方法を説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェントなどがそろった [awesome-copilot][awesome-copilot] リポジトリを確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、コアの `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要に加えて、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、**GitHub Actions Workflows** の指針もあります。これらの指示は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリのルートからの相対 glob を指定しています。たとえば `**/*.test.ts` に一致する TypeScript のテスト ファイルが対象です。
5. このプロジェクトでユニットテストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、既存のプロジェクト ファイルへのリンクがあります。大きな指示を再利用できる小さなファイルに分け、Copilot がコード生成時に参考にできる例を示せます。ここに記載されたパスは、リポジトリのルートではなく指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc のドキュメント コメントとファイル ヘッダーのコメントに関するルールを追加します。

## リポジトリに標準を追加する

この演習で行う編集は、コード内のドキュメント コメントとファイル レベルのコメント ヘッダーを必須にするプロジェクト全体のルールを追加することです。以降の演習で Issue を割り当てるとき、cloud agent はこの指示を使います。

編集する前に、作業用のブランチを準備します。cloud agent は Issue の対象ブランチから指示を読み取りますが、ここでは変更を整理してコミットします。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` はプロジェクト レベルの情報を Copilot に提供します。コードの提案を改善するために、リポジトリのコーディング標準を明記しましょう。

1. `.github/copilot-instructions.md` を開きます。
2. 27行目付近にある **Code formatting requirements** セクションを見つけます。コーディング標準は記載されていますが、コード内のドキュメントに関するルールはまだありません。そのため、生成されたヘルパーにドキュメント コメントがありませんでした。
3. 既存の標準の直後に、ファイル ヘッダーと TSDoc のドキュメント コメントを追加する次の Markdown を追加します。

   ```markdown
   - エクスポートする関数には、目的、パラメーター、戻り値を説明する TSDoc コメントを必ず付けてください。
   - import 文やコードより前に、ファイルの目的を説明するコメント ブロックを追加してください。
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ レベルの `.github/copilot-instructions.md` には全体的な指針を、`*.instructions.md` には特定の言語、ファイル種別、タスク向けの指示を記載できます。ここで追加したドキュメント コメントのルールのように、プロジェクト全体に適用する標準にはリポジトリ レベルのファイルが適しています。

## 指示の更新をコミット、プッシュしてマージする

cloud agent は、Issue が対象とするブランチの指示ファイルを読み込みます。次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が新しい指示を使えるように、変更を `main` にマージする必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを開き、マージします。次の演習で Issue を割り当てると、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチの作成を省略して、変更を `main` にコミットしてもかまいません。ここでは、実際のプロジェクトでの手順を反映するため、ブランチを使っています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。このハーネスの残りの演習で cloud agent が作成するプルリクエストをレビューすると、その標準が反映されていることを確認できます。

次は、[初めての Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
