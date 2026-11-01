---
title: "演習 1 - カスタム instruction (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。[instruction file][instruction-files] は、そのガイダンスを Copilot に渡し、何を実行するかだけでなく、どのように実行するかも伝えます。

この演習では、次のことを行います。

- リポジトリのカスタム instruction とパス単位の instruction file を使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどのように渡るかを確認します。
- `.github/copilot-instructions.md` に、リポジトリ全体に適用される標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、このハーネスでは変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub issue をもとに非同期で作業するため、影響をリアルタイムに並べて示すのが難しいためです。後の演習で cloud agent が作成した pull request をレビューするときに、instruction file の影響を確認します。

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

- `.github/copilot-instructions.md` は、リポジトリに対する**すべて**のリクエストで Copilot に渡される単一の instruction file です。Copilot へのほとんどの chat や CLI リクエストに関係する、プロジェクト レベルのコンテキストを記述します。技術スタック、プロジェクトの概要、ベスト プラクティス、その他の全体的なガイダンスなどが含まれます。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けに作成できます。TypeScript や Astro などの言語、UI component の作成、ユニット テストの追加といったタスクのガイドラインを記述できます。

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

このリポジトリには、中心となる `copilot-instructions.md` と、タスクごとの `*.instructions.md` が用意されています。エディターまたは GitHub の Web UI で確認しましょう。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の **GitHub Actions Workflows** に関するガイダンスも確認してください。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する instruction があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める repo root 相対の glob を指定しています。たとえば、`**/*.test.ts` に一致する任意の TypeScript テスト ファイルが対象になります。
5. このプロジェクトのユニット テスト作成に関する instruction を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾まで確認します。`unit-tests.instructions.md` などの別の instruction file や、プロジェクト内の既存ファイルへのリンクがあります。大きな instruction を小さな再利用可能な単位に分け、Copilot が参考にできる例を示せます。（ここに記載されたパスは repo root ではなく、instruction file からの相対パスです。）

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメント作成はまだ必須ではありません。次の手順で、TSDoc の doc comment とファイル コメント ヘッダーのルールを追加します。

## リポジトリ全体の標準を追加する

次に、コード内のドキュメントを TSDoc の doc comment とファイル レベルのコメント ヘッダーで記述するという、プロジェクト全体のルールを追加します。cloud agent は、この後の演習で issue に取り組むときに、このルールを使います。

編集の前に、作業用のブランチを準備します。cloud agent は issue の対象ブランチから instruction を読み込みますが、変更を整理して commit します。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を渡すファイルです。コーディング標準を記述して、コード提案の質を高めましょう。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。現在の標準は記載されていますが、コード内ドキュメントのルールはないため、生成された helper に doc comment がありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加し、ファイル コメント ヘッダーと TSDoc の doc comment を Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、instruction file は全体的なガイダンス用のリポジトリ レベル (`.github/copilot-instructions.md`) にも、特定の言語、ファイル種別、タスク用の `*.instructions.md` にも作成できます。doc comment ルールのようにプロジェクト全体に適用する標準は、リポジトリ レベルのファイルに記述します。

## instruction の更新を commit、push、マージする

cloud agent は、issue が対象とするブランチの instruction file を読み込みます。次の演習で issue を Copilot に割り当てると、`main` からブランチを作成します。cloud agent が新しい instruction を使えるように、変更を `main` にマージしてください。

1. 変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` への pull request を開き、マージします。次の演習で issue を割り当てると、cloud agent はこの instruction を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチの作成を省略して `main` に直接 commit できます。ブランチを使う手順は、実際のプロジェクトでの方法に合わせています。

## まとめと次のステップ

Copilot がこのプロジェクトの instruction file からコンテキストを取得する仕組みを確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。残りのハーネスで cloud agent が作成する pull request から、この標準が使われている様子を確認します。

次は、[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタム instruction file][instruction-files]
- [カスタム instruction 作成のベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム instruction をよりよく書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
