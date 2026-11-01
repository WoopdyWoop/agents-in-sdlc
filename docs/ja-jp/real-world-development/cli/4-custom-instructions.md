---
title: "レッスン 4 - カスタム instruction で Copilot を導く"
description: "リポジトリの instruction を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要があるなら、そのガイダンスを Copilot が利用できるようにします。[instruction file][instruction-files] では、必要なコードだけでなく、その構成方法も指定できます。フィルタリング機能を実装できたので、Copilot が参照した instruction を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの instruction とパス単位の instruction file が agent にどう渡るかを確認します。
- コーディング標準に従うよう instruction file を更新します。
- instruction file がコードに与える影響を確認します。

## シナリオ

優れた開発チームである Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントにはコードの内容を繰り返すのではなく、意図や自明でない判断を記述します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を付けます。該当する場合は、注入可能な `db` 引数についても記述します。
- 再利用可能な Astro component には `Props` の契約を記述し、関連コードが変更されたときはコメントも最新の状態にします。
- 既存の書式設定と lint のガイダンスを維持します。

instruction file を使うと、Copilot に適切な情報を渡し、ここで取り上げるプラクティスに沿ってタスクを実行できます。

## instruction file

カスタム instruction を使うと、コンテキストや設定を Copilot に渡し、コーディング スタイルや要件を理解させられます。ライブラリやコーディング規約、コメントの書き方などを指定すると、より関連性の高い提案やコード スニペットを得るのに役立ちます。リポジトリ全体に適用する instruction も、特定のファイル向けの instruction も作成できます。

instruction file には、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべて**のリクエストで Copilot に渡される単一の instruction file です。ほとんどのリクエストに関係するプロジェクト レベルの情報を記述します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスク向けのガイドラインを提供します。

> [!NOTE]
> ほかの instruction 形式とそのサポート状況は、環境によって異なります。特定の形式を使用する前に、[カスタム instruction のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム instruction file を確認する

変更の影響を確認できるよう、スターター プロジェクトには instruction file が用意されています。変更を加える前に、既存のファイルを確認しましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡潔な説明とコーディング ガイダンスを確認します。この instruction は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いてファイルを確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する instruction があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、instruction の適用対象を決める glob を指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの instruction file や既存のプロジェクト ファイルへのリンクを確認します。これにより、大きな instruction を小さな再利用可能なファイルに分け、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて instruction file を更新する

すでにあるファイルはよい出発点ですが、追加できるガイダンスがあります。新しく生成した TypeScript に TSDoc コメントを追加するため、基本となる `copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** というセクションを探します。35 行目付近にあります。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの instruction を読み込みます。更新した instruction を保存したら、フィルタリングの会話を終了して再開し、機能のコンテキストを保ったまま新しいガイダンスを読み込ませます。

1. フィルタリングの会話がまだ開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリ ルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択画面が表示されたら、フィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に、更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が instruction からコンテキストを取得する仕組みを確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの instruction とパス単位の instruction file が agent にどう渡るかを確認しました。
- コーディング標準に従うよう instruction file を更新しました。
- instruction file がコードに与える影響を確認しました。

次は、[再利用可能な quality-checks skill をカスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI 用のカスタム instruction を追加する][instruction-files]
- [カスタム instruction のサポート][custom-instructions-support]
- [カスタム instruction を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
