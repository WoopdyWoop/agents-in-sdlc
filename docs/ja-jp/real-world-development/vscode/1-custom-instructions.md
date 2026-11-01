---
title: "演習 1 - カスタム instruction (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。Copilot に具体的なコンテキストを渡す方法はいくつかあります。その中でも重要なのが、コード生成の指針を伝える [instruction file][instruction-files] です。

この演習では、次のことを行います。

- リポジトリのカスタム instruction とパス単位の instruction file を使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどのように渡るかを確認します。
- *現在の* instruction を使って、フィルタリングの最初のデータ部分（publishers helper）を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準に従うことを確認します。
- instruction の更新とフィルタリングの部分を `main` に commit し、次の演習で Copilot が更新したガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが、設定した標準から外れる場合があります。Copilot は非決定的に動作します。この演習の目的は、instruction を更新した後の傾向の変化を確認することであり、出力を 1 文字ずつ一致させることではありません。

## Instruction file

### シナリオ

優れた開発チームである Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データ レイヤーには、必ずユニット テストを用意します。
- UI はダーク モードにし、現代的な印象にします。
- TSDoc の doc comment を使って、コードにドキュメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

instruction file を使って、Copilot が適切な情報に基づき、これらのプラクティスに沿って作業できるようにします。

### カスタム instruction

カスタム instruction を使うと、コンテキストや設定を Copilot に渡し、コーディング スタイルや要件を理解させられます。コーディング規約、ライブラリ、コメントの書き方などを指定すると、より関連性の高い提案やコード スニペットを得るのに役立ちます。リポジトリ全体に適用する instruction も、特定のファイル向けの instruction も作成できます。

instruction file には、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべて**のリクエストで Copilot に渡される単一の instruction file です。技術スタック、プロジェクトの概要、ベスト プラクティス、その他の全体的なガイダンスなど、Copilot へのほとんどの chat や CLI リクエストに関係する情報を記述します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けに作成できます。TypeScript や Astro などの言語、UI component の作成、ユニット テストの追加などのガイドラインを提供できます。

> [!NOTE]
> IDE で作業する場合、instruction file は Copilot Chat のコード生成にのみ使われ、コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの instruction file と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの規格に基づく instruction file][custom-instructions-support] もサポートしています。

### Instruction file を管理するためのベスト プラクティス

instruction file の作成方法を詳しく説明することは、このワークショップの範囲外です。サンプル プロジェクトの例は、一般的なアプローチを示しています。

- `copilot-instructions.md` には、作成中のもの、プロジェクトの構造、全体に適用するコーディング標準など、プロジェクト レベルのガイダンスを記述します。
- `*.instructions.md` は、ユニット テスト、Astro component、データ レイヤーなどのファイル種別や、特定のタスク向けの具体的な instruction に使います。
- 自然な言葉で、明確なガイダンスを記述します。望ましいコードと望ましくないコードの例を示します。

instruction file の作成方法にも、AI の使い方にも唯一の正解はありません。プロジェクトに合った方法を試しながら見つけましょう。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した instruction file を用意しましょう。このプロジェクトのファイルを確認すると、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けのファイルがあります。
>
> Copilot に instruction file を作成させることもできます。各 surface での手順は異なります。たとえば VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。詳細は、現在使っている surface のレッスンで説明します。
>
> テンプレートや出発点を探す場合は、instruction file、カスタム エージェントなどがそろったリポジトリ [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム instruction file を確認する

このリポジトリには、中心となる `copilot-instructions.md` と、タスクごとの `*.instructions.md` が用意されています。エディターまたは GitHub Web UI で確認しましょう。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の **GitHub Actions Workflows** に関するガイダンスも確認してください。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する instruction があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める repo root 相対の glob を指定しています。たとえば `**/*.test.ts` に一致する任意の TypeScript テスト ファイルが対象になります。
5. このプロジェクトのユニット テスト作成に関する instruction を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾まで確認します。`unit-tests.instructions.md` などの別の instruction file や、プロジェクト内の既存ファイルへのリンクがあります。大きな instruction を小さな再利用可能な単位に分け、Copilot が参考にできる例を示せます。（ここに記載されたパスは repo root ではなく、instruction file からの相対パスです。）

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメント作成はまだ必須ではありません。次の手順で、TSDoc の doc comment とファイル コメント ヘッダーのルールを追加します。

## 変更用のブランチを作成する

ベスト プラクティスに従い、作業用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 次のコマンドで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## instruction を更新する*前に* Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

カスタム instruction の影響を確認するため、まずは現在の instruction file を使ってプロンプトを送信します。その後に instruction を更新して同じプロンプトを再送信し、違いを確認します。

1. 前の演習のエディター タブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. `src/lib/publishers.ts` を開き、helper の配置場所を Copilot に知らせます。
3. Chat ビューの agent ドロップダウンから **Agent** を選択し、ファイルの変更を適用できるようにします。

   ![Chat ビューで agent picker を選択するスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べ、コードを更新します。helper とテストのファイルが変更されることがあります。
6. 提案された helper が型付き関数であり、最初の引数に `db` client を受け取り、型付き publishers 配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約によるものです。
7. 提案されたコードに TSDoc の doc comment とファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指定しなくても doc comment が追加されることがあります。その場合も問題ありません。instruction の更新後に一貫性が高まることが重要です。

## リポジトリ全体の標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を渡すファイルです。コーディング標準を記述して、コード提案の質を高めましょう。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。現在の標準は記載されていますが、コード内ドキュメントのルールはありません。そのため、生成された helper に doc comment がありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加し、ファイル コメント ヘッダーと TSDoc の doc comment を Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、instruction file は全体的なガイダンス用のリポジトリ レベル (`.github/copilot-instructions.md`) にも、特定の言語、ファイル種別、タスク用の `*.instructions.md` にも作成できます。doc comment ルールのようにプロジェクト全体に適用する標準は、リポジトリ レベルのファイルに記述します。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択してコンテキストをクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot に対象ファイルを知らせます。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc comment があることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準に従うよう Copilot を導き、次の演習の土台となる実際のコードに適用できました。

## instruction を commit してブランチを push する

instruction file も、ほかのアセットと同様に source control で管理します。変更を commit してリポジトリへ push しましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. 次のコマンドを実行して、instruction と helper の変更を確認します。

   ```bash
   git status
   ```

3. instruction の更新と helper のために Copilot が変更したすべてのファイルを stage して commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の変更 (例: テスト) がある場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## pull request を作成してマージする

ブランチを push できたので、バックログのドキュメント標準 issue に紐づけた pull request を作成します。GitHub のツールはプロジェクトにすでに接続されているため、Copilot に issue を探し、issue を解決する PR を作成してマージするよう依頼できます。

> [!NOTE]
> この演習では、Copilot に初めて GitHub 上の操作を依頼します。プロジェクト テンプレートでは GitHub のツールが workspace に接続済みです。Copilot が初めてツールを使うときに、VS Code で GitHub へのサインインを求められることがあります。案内に従って認証してください。後の演習では、Model Context Protocol による接続方法を学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、agent picker で **Agent** が選択されていることを確認します。
3. instruction file の更新に関する issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。PR には、instruction の更新と publishers helper の土台の両方を記述するよう伝えます。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探して PR を作成します。
5. GitHub の操作を許可するよう求められたら、コマンドを確認して、適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージし、ローカル ブランチを `main` に戻すよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作や shell command の実行を許可するよう求められたら、コマンドを確認して、適切な場合に **Allow** を選択します。

GitHub Copilot の支援で pull request を作成してマージできました。

## まとめと次のステップ

Copilot がこのプロジェクトの instruction file からコンテキストを取得する仕組みを確認しました。VS Code の Copilot Chat を使って、次のことを行いました。

- *既存の* instruction を使ってコード生成を依頼し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認しました。
- instruction の更新と helper の土台を `main` に commit し、次の演習で使えるようにしました。

次は、Copilot がコードベース全体に新機能を追加するときに [agent mode で instruction を使います][next-lesson]。

## リソース

- [GitHub Copilot のカスタム instruction file][instruction-files]
- [カスタム instruction 作成のベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム instruction をよりよく書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム instruction][personal-instructions]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
