---
title: "演習 2 - Copilot agent mode で新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は、必要なファイルを探して変更し、テストを実行し、問題を修正し、変更漏れがあればさらに編集する、といった作業を繰り返します。

ここで役立つのが Copilot agent mode です。

Copilot agent mode は IDE 内で、より自律的に作業できるよう設計されています。開発者と同様に、プロジェクト構造を調べ、必要な変更を行い、テストなどのタスクを実行して、見つかった問題を自動的に修正します。agent mode を使って、サイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> agent mode と cloud agent は名前が似ていますが、異なる体験を提供します。Agent mode は IDE 内で作業し、素早くやり取りしながら変更できます。Cloud agent はチームの一員のように非同期で作業し、issue と pull request を介してやり取りします。

この演習では、次のことを学びます。

- Copilot agent mode がプロジェクトを調べ、関連ファイルを特定し、複数のファイルを連携して変更する方法。
- GitHub Copilot agent mode が UI とデータ レイヤーにわたって機能を実装する方法。
- Copilot agent mode が生成した変更とテストを、コードベースにマージする前にレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、利用者がカテゴリと発行元で絞り込めるようにします。前の演習ですでに publishers helper を追加しました。Copilot agent mode を使って、残りのデータ レイヤー、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを起動する

変更を加える前に、Tailspin Toys の Web サイトを開いて現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボード ゲームのクラウドファンディング プラットフォームです。ゲームを一覧表示し、その詳細を確認できます。単一の Astro app であり、ビルド時にページを静的 HTML として生成します。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite データベースを直接参照します。独立したバックエンド API やクライアント側 UI framework はありません。再利用可能なデータ アクセス helper は `src/lib/` にあり、インタラクティブな機能は標準 DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを起動するには、単一の Astro dev server を開始する `npm run dev` task を使います。

1. Codespace に戻ります。前の演習を終えて `main` に戻っているはずです。
2. Codespace で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. `main` を更新し、フィルタリング作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro dev server の準備ができると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くか尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで Astro server のアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost URL のリンクを選択すると、`https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは、Web server を実行している Codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、次の主な機能を確認できます。

- **Home Page**: ボード ゲームのタイトル、画像、説明を一覧表示します。
- **Game Details Page**: ゲームを選択すると、そのタイトル、説明、発行元、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して改善します。まずバックログを確認しましょう。テンプレートからプロジェクトを作成したときに issue のバックログも作成されているので、GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューの agent ドロップダウンから **Agent** を選択します。Agent は複数のファイルにわたって自律的に計画・実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューで agent picker を選択するスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使ったため、手順に含めています。提案の一貫性を高め、学習しやすくするためです。ただし、LLM は確率的に動作するため、手順の例とは異なる提案が表示される場合があります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot は別の MCP command を使う場合がありますが、タスクは完了できます。

3. 次のプロンプトを Copilot に送信し、issue のバックログを尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示する command を実行します。
5. 生成された issue の一覧を確認します。

Copilot が利用者にとって有用だと考える順に issue を優先順位付けしたことを確認してください。

## Instruction file を確認する

agent にコードを生成させる前に、Copilot にコンテキストを渡す instruction file を確認します。Web サイトの機能追加方法に関する情報を含む [UI instruction file](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - component 設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle など、ファイル種別ごとの instruction へのリンク。

> [!TIP]
> instruction file から、別の instruction file やプロジェクト内のファイルを参照できます。パスは instruction file の場所からの相対パスです。大きな instruction を小さな再利用可能な単位に分け、Copilot に例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリングを完成するには、少なくとも 3 つの変更が必要です。

- データ レイヤー (`src/lib/`) のフィルタリング ロジックを追加または調整する。
- フィルタリングの動作をテストするテストを追加または更新する。
- ゲーム一覧ページにフィルタリング UI を追加する。

コードベースにマージする前に、テストを実行して成功させる必要があります。Copilot agent mode にこれらの作業を依頼しましょう。

1. 現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択して、instruction file を追加します。

   ![UI instruction file を選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューの agent ドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューで agent picker を選択するスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルとして引き続き **Claude Sonnet 4.5** が選択されていることを確認します。
5. 次のプロンプトで、バックログの関連 issue に基づいて機能を実装するよう Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot はプロジェクトを調べ、機能に関連するファイルを探します。データ レイヤー helper、UI、テストを見つけて変更し、テストを実行します。

   ![Copilot がプロジェクト ファイルを調べるスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調べ、ファイルを変更してテストを実行します。タスクやコードベースの複雑さによっては数分かかります。作業中、エディターに **Keep** と **Undo** が表示される場合があります。Copilot が完了すると、すべての変更に対して **Keep** または **Undo** を選択できます。作業の途中で選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインにテスト実行の確認が表示されたスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、一部のテストが失敗したりする場合があります。問題ありません。Copilot はコード生成とテストを繰り返し、タスクが完了してエラーが見つからなくなるまで作業します。

   ![Copilot agent mode の Chat セッションが完了したスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を試します。
11. 動作とコードのレビューが終わったら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot agent mode を使って、Tailspin Toys の Web サイトに機能を追加する方法を学びました。次のことを確認しました。

- GitHub Copilot agent mode が UI とデータ レイヤーにわたって新しい機能を実装する方法。
- Copilot agent mode がプロジェクトを調べ、関連ファイルを見つけ、複数の変更を行う方法。
- Copilot agent mode が生成した変更とテストをマージ前にレビューする方法。

次は [Playwright MCP server で機能をテストし][next-lesson]、pull request を作成します。

### ボーナス演習 — ページネーションを実装する

ゲーム一覧が増えると、ページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションを追加するよう依頼します。次の点を検討してください。

- 既存の instruction file を使うなど、既存のベスト プラクティスに従います。
- ページサイズを利用者が選択できるようにするか、固定にするかなど、ページネーションの動作を決めます。
- 希望どおりの実装になるよう、必要なガイダンスをプロンプトに含めます。
- 必要に応じて Copilot に変更を依頼し、追加のコンテキストを渡して反復します。これは Copilot を使う際の通常の作業です。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
