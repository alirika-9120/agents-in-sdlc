---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使ううえで、コンテキストは重要です。タスクを特定の方法で行う必要がある場合や、Copilot に背景情報を伝える必要がある場合は、そのコンテキストを利用できるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。特に重要なのが、コード生成の指針を与える[指示ファイル][instruction-files]です。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパス指定の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- 既存の指示のまま、フィルター機能用の最初のデータ層 (パブリッシャー ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトをもう一度実行し、生成コードに新しい標準が反映されることを確認します。
- 指示の更新とフィルター機能の一部を `main` にコミットし、次の演習で Copilot が更新されたガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準の一部に従わない場合があります。Copilot の動作は確定的ではありません。この演習では、指示の更新後に出力の傾向が変わることを確認します。コードを文字単位で同じにすることが目的ではありません。

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

このリポジトリに用意されている、コアの `copilot-instructions.md` とタスク別の `*.instructions.md` ファイルを確認します。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要に加えて、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、**GitHub Actions Workflows** の指針もあります。これらの指示は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリのルートからの相対 glob を指定しています。たとえば `**/*.test.ts` に一致する TypeScript のテスト ファイルが対象です。
5. このプロジェクトでユニットテストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、既存のプロジェクト ファイルへのリンクがあります。大きな指示を再利用できる小さなファイルに分け、Copilot がコード生成時に参考にできる例を示せます。ここに記載されたパスは、リポジトリのルートではなく指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc のドキュメント コメントとファイル ヘッダーのコメントに関するルールを追加します。

## 変更用のブランチを作成する

ベスト プラクティスに沿って、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。演習ごとにモードとモデルの選択が異なります。必要な場合は、各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルのままプロンプトを送信します。その後、指示を更新して同じプロンプトを再送信し、違いを確認します。

1. 前の演習から開いているエディター タブを閉じ、Copilot が必要なコンテキストだけを取得できるようにします。
2. Copilot がヘルパーの配置場所を把握できるよう、`src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェント選択メニューで **Agent** を選択し、Copilot がファイルを編集できるようにします。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   src/lib/publishers.ts を新規作成または更新し、すべてのパブリッシャーについて名前と ID を返すデータ アクセス ヘルパーを追加してください。ファイルに変更を適用してください。
   ```

5. Copilot がプロジェクトを調べ、ヘルパー ファイルとテストを含むコードを変更します。
6. 生成されたヘルパーが、最初の引数に `db` クライアントを受け取り、型付きのパブリッシャー配列を返す関数であることを確認します。これは `.github/instructions/drizzle.instructions.md` のデータレイヤー規則 (`src/lib/*.ts` に適用) によるものです。
7. 生成されたコードに TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の動作は確定的ではないため、指示がなくてもドキュメント コメントを追加する可能性があります。その場合も問題ありません。指示を更新した後の一貫性の向上を確認することが目的です。

## リポジトリに標準を追加する

前述のとおり、`.github/copilot-instructions.md` はプロジェクト レベルの情報を Copilot に提供します。コードの提案を改善するために、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27行目付近にある **Code formatting requirements** セクションを見つけます。コーディング標準は記載されていますが、コード内のドキュメントに関するルールはまだありません。そのため、生成されたヘルパーにドキュメント コメントがありませんでした。
3. 既存の標準の直後に、ファイル ヘッダーと TSDoc のドキュメント コメントを追加する次の Markdown を追加します。

   ```markdown
   - エクスポートする関数には、目的、パラメーター、戻り値を説明する TSDoc コメントを必ず付けてください。
   - import 文やコードより前に、ファイルの目的を説明するコメント ブロックを追加してください。
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ レベルの `.github/copilot-instructions.md` には全体的な指針を、`*.instructions.md` には特定の言語、ファイル種別、タスク向けの指示を記載できます。ここで追加したドキュメント コメントのルールのように、プロジェクト全体に適用する標準にはリポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をクリアします。
2. `src/lib/publishers.ts` を選択して、Copilot が正しいファイルを参照するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   src/lib/publishers.ts を新規作成または更新し、すべてのパブリッシャーについて名前と ID を返すデータ アクセス ヘルパーを追加してください。ファイルに変更を適用してください。
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディング プラットフォームのパブリッシャー データ アクセス ヘルパー。
    * データベースからパブリッシャー情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * すべてのパブリッシャーについて ID と名前の一覧を返します。
    *
    * @param db - Drizzle データベース クライアント。
    * @returns パブリッシャー オブジェクトの配列に解決する Promise。
    */
   ```

Copilot に新しいプロジェクト標準を伝え、次の演習で使う実際のコードに適用できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルもリポジトリ内のほかのファイルと同じように、ソース管理で管理します。指示とヘルパーの変更をコミットしてブランチをプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナル ウィンドウを開きます。
2. ターミナルで次を実行し、指示とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の更新と、ヘルパーの基盤として Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、プルリクエストを作成して、バックログのドキュメント標準に関する Issue に関連付けます。手動で行う代わりに、すでにプロジェクトに接続されている GitHub ツールを使って Copilot に依頼します。Issue を探し、PR でクローズし、PR をマージするよう指示します。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのは、これが初めてです。プロジェクト テンプレートで GitHub のツールがあらかじめ接続されています。Copilot が初めてツールを使うときに、VS Code で GitHub へのサインインを求められる場合があります。プロンプトに従って許可してください。この接続 (Model Context Protocol) については、後の演習で学びます。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成して、指示の更新と次のフィルター機能に向けたパブリッシャー ヘルパーの基盤の両方を説明するよう Copilot に依頼します。

   ```plaintext
   指示ファイルの更新に関する Issue を探してください。現在のブランチから新しい PR を作成し、その Issue をクローズすることを明記してください。また、今後のフィルター機能に向けてパブリッシャー ヘルパーの基盤も追加したことを記載してください。
   ```

4. Copilot が Issue を探して PR を作成します。
5. Copilot が GitHub 操作の許可を求めたら、コマンドを確認して、適切であれば **Allow** を選択します。
6. PR が作成されたら、Copilot にマージしてローカル ブランチを `main` に戻すよう、次のプロンプトで依頼します。

   ```plaintext
   PR を main にマージしてください。その後、ローカルで main に戻り、最新コードを pull して最新の状態にしてください。
   ```

7. Copilot が GitHub 操作とシェル コマンドの許可を求めたら、内容を確認して適切であれば **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認しました。VS Code の Copilot Chat を使って、次の操作を行いました。

- 既存の指示のままコード生成プロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認しました。
- 指示の更新とヘルパーの基盤をコミットし、次の演習で使えるよう `main` にマージしました。

次は、Copilot がコードベース全体に新機能を追加する[エージェント モード][next-lesson]で、指示を実際に使います。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
