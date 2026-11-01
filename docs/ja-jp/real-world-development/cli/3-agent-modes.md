---
title: "レッスン 3 - Agent モード: Plan と Autopilot"
description: "Plan でアプローチを合意し、Autopilot で issue からフィルタリングを実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初にプロジェクトへ小さな機能を追加しました。しかし、より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は、組織の既存の開発フローに沿って適切な方法で実装できるよう設計されています。このレッスンから、agent を活用する一般的な開発プロセスをいくつかのレッスンにわたって実践します。まず issue から機能を作成し、コードの妥当性と機能の動作を確認したうえで、最終的にプロジェクトへマージします。

> [!NOTE]
> 機能開発を続ける間は、同じ会話とブランチを使います。通常は作業するファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主な概念に集中するため簡略化しています。

このレッスンでは、次のことを行います。

- GitHub issue から Copilot の新しい会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで新しい機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

この機能の開発を続けながら、リポジトリの instruction を更新し、既存の `quality-checks` skill をカスタマイズし、MCP による検証を追加して QA agent を作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、利用者がカテゴリや発行元でゲームを絞り込める必要があります。バックログの issue には機能の説明がありますが、カテゴリを組み合わせる方法など、実装前に合意すべき点があります。Plan モードで仕様を決め、その後 Autopilot に範囲を限定した実装を任せます。

## 背景

AI コーディング agent を開発フローに取り入れても、基本は変わりません。むしろ、基本がより重要になります。多くの開発者は、次のような流れで作業します。

1. 必要な作業を記述した issue から始めます。
2. 実装する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. pull request (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 詳細な手順はチームや組織によって異なりますが、多くの場合、上記の流れを基本にしています。

この標準的な方法に従うことで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じレビュー プロセスを通るようにできます。

## 会話モード

**会話モード**は、agent にどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: agent と一緒に作業します。agent は変更を提案し、次に進む前に入力を待ちます。
- **Plan**: agent は最初に計画を作成し、プロジェクト ファイルを編集できません。
- **Autopilot**: agent はコードを作成し、テストを実行して、タスクが完了するまで自律的に反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## issue から始める

フィルタリングの作業を始める前に、Codespace に戻り、リポジトリとターミナルが準備できていることを確認します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルタリング作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の issue や pull request (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てることで、機能の実装やタスクの実行を Copilot に任せる前に、アプローチを定義できます。複雑な作業では、少し時間をかけて計画するのがお勧めです。Plan モードに切り替え、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 先ほど追加した issue の参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適なアプローチを検討して計画を作成します。

3. フィルタリングの動作について質問されたら、自分の希望に基づいて回答します。ここでは、唯一の正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上下にスクロールしてレビューします。
6. 自分の判断と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューできたら、実装を始めます。Autopilot を使って Copilot に作業を任せましょう。

Autopilot を使うと、Copilot は問題が解決したと判断するまで作業を反復します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みバージョンで同様の名前が付いた選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があります。先に計画をレビューしてください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェック完了と判断する前にセットアップの問題を解決してください。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同じく、マージ前にレビューする必要があります。コードをレビューし、サイトを起動して動作を確認しましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** または **Autopilot** が表示されていないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. 結果を issue と計画時に決めた内容に照らし合わせます。
4. コードのレビューが終わったら、<kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新しい機能を試す

コードがよさそうでも、実際に動くか確認しましょう。前と同じようにアプリを起動し、Codespaces で転送されたポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画で合意したカテゴリ フィルター、発行元フィルター、およびその組み合わせを試します。
4. リセット時と結果が空の場合の動作が、issue と合意内容に一致することを確認します。
5. Codespace に戻り、起動した開発サーバーを Copilot に停止してもらいます。

## まとめと次のステップ

会話モードを切り替えて機能を作成し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から Copilot の新しい会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、コードが文書化されたプラクティスに従うよう、[カスタム instruction を使って][next-lesson]詳しく見ていきましょう。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI のコマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
