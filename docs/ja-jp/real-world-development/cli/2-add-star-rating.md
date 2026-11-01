---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "Copilot CLI でゲーム カードに小さな変更を加え、転送されたブラウザーで確認し、最初の pull request としてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試したので、次はプロジェクトに最初の変更を加えます。小さな変更から始めましょう。ゲームのデータには星評価がすでにありますが、ホーム ページのゲーム カードには表示されていません。agent に評価を表示させ、変更をレビューして、最初の pull request としてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで、目的を絞った Copilot の会話を開始します。
- agent にプロジェクトの小さな変更を依頼します。
- `/diff` で変更をレビューします。
- 転送されたブラウザーでアプリを実行し、変更を確認します。
- 最初の pull request を作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価を設定でき、ゲーム詳細ページにはすでに表示されています。一方、ホーム ページのゲーム カードには、タイトル、カテゴリ、発行元、説明しか表示されていません。ウォームアップとして、agent に既存の評価を各カードへ表示させます。小さく完結した変更なので、最初のセッションにぴったりです。

## 会話の構成

**会話**では、Copilot CLI を使ってタスクに取り組みます。Copilot app とは異なり、通常の CLI 会話では、専用の worktree を作るのではなく、ターミナルで現在チェックアウトしているリポジトリと Git ブランチを使います。保存した会話を後から再開できますが、ファイルとブランチはディスク上の通常の Git 状態のままです。

会話では、プロンプトと agent の応答、ファイルを調査・編集する agent のツール操作、`/diff` で確認できる変更の 3 つを確認できます。

## 会話を開始して変更を依頼する

機能の実装を始めるため、新しい会話を開始します。

1. Codespace に戻ります。
2. まだターミナルを開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. まだ起動していない場合は、次のコマンドで Copilot を起動します。

   ```bash
   copilot --yolo
   ```

4. slash command `/new` を実行して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲーム詳細の表示に使われているファイルを見つけて、必要なコードを作成します。Copilot CLI で新機能を追加できました。

## 差分をレビューする

小さな変更でも、AI が生成したものはすべてマージ前にレビューします。Copilot CLI で変更を確認しましょう。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. 評価がある場合は数値が、`starRating` が `null` の場合は `No rating yet` がゲーム カードに表示されることを確認します。
3. 両方の状態がテストされていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を終了します。

> [!NOTE]
> Copilot を含む生成 AI ツールは確率的に動作するため、生成されるコードは毎回異なる場合があります。特定の実装と一致することではなく、動作を確認してください。

## 変更を確認する

コードを読んだだけで動作すると判断するのではなく、Copilot に Web サイトを起動してもらい、Codespaces で転送されたブラウザーから更新済み UI を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. ゲーム カードに 5 段階の評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初の pull request を作成してマージする

機能を作成できました。次は pull request (PR) を作成し、新しいコードをプロジェクトにマージします。

1. 既定の agent に、変更の commit を依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存の commit を push し、PR の URL を表示します。
3. Copilot CLI に表示された URL を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して PR を開きます。
4. 変更されたファイルとチェック結果を確認します。
5. 準備ができたら **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って、最初の変更をリリースできました。具体的には、次のことを行いました。

- 機能ブランチで、目的を絞った Copilot の会話を開始しました。
- agent にゲーム カードの小さな変更を依頼しました。
- `/diff` で変更をレビューしました。
- 転送されたブラウザーでアプリを実行し、星評価を確認しました。
- 最初の pull request を作成してマージしました。

次は、[フィルタリングの issue から始め、Plan と Autopilot の各モードを使って][next-lesson]、より大きな機能を作成します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI のコマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
