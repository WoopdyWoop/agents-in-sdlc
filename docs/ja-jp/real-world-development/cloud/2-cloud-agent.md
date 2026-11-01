---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム instruction][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能の要望などがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加といったタスクを自律的に実行できます。作業が完了すると、人間の開発者がレビューできる draft PR を作成します。面倒な作業を任せて開発を加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent では、次のことを学びます。

- コード生成環境をカスタマイズする。
- 操作を安全に行う。
- issue の範囲を明確にする重要性。
- issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には解消したい技術的負債があります。最初のサイトを開発した委託先は、ドキュメントを適切に整備していませんでした。実際、ドキュメントがほとんどありません。まず、アプリケーションのエクスポート関数すべてに TSDoc の doc comment を追加します。

さらに、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示し、利用者がほかのゲームも引き続き見られるようにします。現時点では、UX の受け入れテストに使える程度の実装が必要です。この作業は現在ブロッカーですが、ほかに優先度の高い issue があります。

どちらも、優先度が下がりやすいタスクの例です。Copilot cloud agent に割り当てれば、非同期で作業を進めてもらえます。その間、別のタスクに取り組み、後で Copilot の結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。[GitHub issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)方法などがあります。割り当てると Copilot は進捗を追跡する draft pull request を作成し、環境をセットアップしてタスクを開始します。作業中でも完了後でもセッションを確認できます。提案された解決策のレビュー準備ができると、pull request で通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単そうな作業でも、詳しく見ると複雑さがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして扱い、段階を踏んで学び、試し、状況に応じて調整しましょう。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム instruction

前の演習では、カスタム instruction と Copilot への影響を確認しました。カスタム instruction の演習をすでに行った場合は `.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、cloud agent に作業を割り当てる前に、このリポジトリにある instruction file を読み取り専用で確認してください。後でその影響を確認できます。

GitHub Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc の doc comment に関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter を確認します。この instruction は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てたら、作成された pull request に TSDoc の doc comment、TypeScript の規約、コメント ヘッダーがあるか確認してください。これらの内容は instruction file によるものです。後の演習で PR をレビューするときに、改めて確認します。

## Copilot cloud agent の開発環境をセットアップする

コードの作成には、通常、適切な環境と初期セットアップが必要です。ソフトウェア エンジニアのように作業する Copilot にタスクを割り当てる場合も同じです。

Cloud agent は [GitHub Actions][github-actions] を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に設定した[特別なセットアップ ワークフロー][setup-workflow]を作成すると、作業前に必要な開発ツールや依存関係をインストールできます。ラボの流れを円滑にして学習できるよう、このファイルはあらかじめ設定されています。単一の Astro app のために、Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー、移行と seed が済んだ SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 必要な手順に対して、可能な限り最小限の権限を設定します。Copilot は操作用の独自 token を使います。
    permissions:
      # セットアップ手順でリポジトリを clone して依存関係などをインストールする場合は、contents: read が必要です。clone しない場合は、手順の完了後に Copilot が自動的に clone します。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / app のセットアップ (アプリ全体は Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 単一 Astro app のビルドとテスト用に、ローカル SQLite データベースを移行して seed します。
      - name: Set up the database
        run: npm run db:setup
```

このファイルは通常の GitHub workflow と似ていますが、いくつか重要な点があります。

- `copilot-setup-steps` という 1 つの job があり、Copilot が pull request の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` が設定されているため、リポジトリの **Actions** タブから手動で実行できます。Copilot の実行を待たずに workflow の動作を確認できます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、古いままのプロジェクトや、ドキュメントが不足しているプロジェクトは少なくありません。このような技術的負債は解消されず、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成を得意とするため、cloud agent に割り当てるのに適した issue です。バックグラウンドでドキュメントを作成してもらい、後の演習で結果を確認します。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue の画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当て画面を開きます。

  ![issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成された pull request (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前になります。PR が一覧にない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が todo list を作成したことを確認します。

> [!NOTE]
> Copilot の todo list が PR に表示されるまで数分かかることがあります。Copilot が環境を作成し、先ほどの workflow を実行して、プロジェクトを分析し、問題に取り組む方法を決めています。

13. todo list を確認し、実行予定のタスクを読みます。
14. pull request のタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションを表示する](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されない場合は、ブラウザーを更新してください。

16. ライブ セッションをスクロールし、Copilot の作業を確認します。コードを調べて状態を理解し、方針を検討してからコードを作成する過程が表示されます。

処理には数分かかります。Copilot cloud agent の主な目的は、タスクを非同期で実行し、その間にほかの作業を続けられるようにすることです。別のタスクも cloud agent に割り当て、その間にアプリの機能を実装する作業を進めましょう。

## ゲーム詳細ページに関連ゲームを追加する

Copilot cloud agent の利点の 1 つは、作業を分けられることです。デザイン チーム向けに関連ゲームを追加する作業に時間がかからないとしても、その時間を別の作業に使えます。cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue の画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当て画面を開きます。

  ![issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

その後、issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことが分かります。

![issue で作業中の Copilot を示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 件目の依頼に取り組んでいます。cloud agent はソフトウェア エンジニアと同じように作業するため、常時監視する必要はなく、完了後にレビューできます。次はカスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に issue を割り当てると、非同期で作業してもらえます。技術的負債の解消、新機能の作成、別のフレームワークからのコード移行などに利用できます。

このレッスンでは、次のことを学びました。

- コード生成環境をカスタマイズする。
- 操作を安全に行う。
- issue の範囲を明確にする重要性。
- issue を Copilot に割り当てる。

cloud agent がバックグラウンドで作業している間に、カスタム エージェントを作成して使う方法を確認しましょう。[Copilot cloud agent は MCP server も使用できます][cloud-agent-mcp]。また、前のモジュールで確認したカスタム instruction も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
