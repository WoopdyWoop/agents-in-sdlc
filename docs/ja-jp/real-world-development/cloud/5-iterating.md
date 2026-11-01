---
title: "演習 5: GitHub Copilot の作業を反復改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: Agent の管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能追加を目的に、GitHub Copilot といくつかのタスクに取り組みました。コードにドキュメントを追加し、デザイン チームが反復改善するための関連ゲーム機能を作成し、ハイ コントラストとライト モードの切り替えを含むアクセシビリティ機能を実装するよう依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを伝えて改善しましょう。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全体にレビューを依頼する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、監視されずにタスクを実行するため、安全性を維持するための制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みアクセスは作業に使うブランチに**限られます**。
- Cloud agent は GitHub Actions 内で、独立した一時的な環境を作成して作業します。
- GitHub Actions のワークフローを実行するには、人間の承認が必要です。
- 既定では、[外部リソースへのアクセスは制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP server も対象です。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初の pull request (PR)、つまりコードへのドキュメント追加をレビューします。GitHub.com の標準 PR インターフェイスを使います。

> [!NOTE]
> PR に、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は、外部 MCP server への呼び出しを含む外部リソースへのアクセスが既定で制限されているため、これは**想定される動作**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が付いています。その場合は完了まで待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選び、変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc の doc comment など、新たに追加されたドキュメントを含むコードを確認します。具体的な変更内容は異なる場合があります。Exercise 2 の最初に確認したカスタム instruction file による TSDoc、TypeScript の規約、コメント ヘッダーがあるか確認してください。
6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが追加されます。問題がなければ、Astro app のプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

pull request で Copilot と作業するのは、一方通行ではありません。チーム メンバーに依頼するのと同じように、PR またはコードのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、変更に対応する新しいセッションを開始します。結果は確率的に変わるため、決まった依頼文はありません。次のような変更を依頼できます。

- 各コード ファイルの先頭に、役割を説明する短いコメント ヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc の doc comment を追加する。
- Astro app の構造を説明する README を作成する。

1. 上記の例などを使い、コードベースに必要なドキュメント変更を **@copilot** にメンションしてコメントで依頼します。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot は変更のための新しいセッションを開始します。
3. **Back to pull request** を選択すると PR に戻れます。

    ![PR に戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、PR に新しい commit が追加されます。
5. **Files changed** タブを選択し、変更をレビューします。

満足するまで反復改善できます。準備ができたら、PR の draft を解除してレビュー可能にし、`main` にマージします。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue を解決するため、Copilot が作成した PR を確認しましょう。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を開きます。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションにワークフローが追加されます。問題がなければ、Astro app のプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名（例: **copilot/fix-8**）に置き換えてください。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に追加の変更を依頼できます。たとえば、表示する関連ゲームの数を調整したり、コメント ヘッダーと TSDoc の doc comment を追加したりできます（この issue を割り当てた時点では、カスタム instruction はまだ更新していません）。前と同様に **Conversation** タブにコメントを追加すると、Copilot が新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用のカスタム エージェントを使って実装した機能を確認します。この PR には、Exercise 3 で依頼したハイ コントラスト モードと、Exercise 4 の Mission control で追加を依頼したライト モードが含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が付いています。その場合は完了まで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点を確認します。
   - モードを切り替える UI component
   - ユーザー設定を保持する local storage の使用
   - ハイ コントラスト モードとライト モードの CSS やスタイル変更
   - アクセシビリティ属性（ARIA label、キーボード操作など）
   - モード切り替えを管理する JavaScript/TypeScript コード
6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが追加されます。問題がなければ、Astro app のプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名に置き換えてください。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリを起動し、ブラウザーでハイ コントラスト モードとライト モードを試します。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ用のカスタム エージェントが、Copilot をアクセシビリティのベスト プラクティスに沿って導いたことに注目してください。アクセシビリティ上の問題や改善点があれば、前の PR と同様に **@copilot** にコメントで変更を依頼できます。

## 任意の演習 — 委任を続ける

Cloud agent は、実際のバックログ項目を割り当て、その間に別の作業を進めるときに効果を発揮します。リポジトリに issue を追加して Copilot に割り当て、使い方に慣れましょう。次のような例があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス helper に入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンを通じて、次のことを行いました。

- リポジトリに含まれるカスタム instruction file を確認し、cloud agent の出力に及ぼす影響を確認しました。
- issue を Copilot cloud agent に割り当て、環境のセットアップ、計画、非同期の実行を確認しました。
- アクセシビリティ用のカスタム エージェントを作成して使い、ハイ コントラストとライト モードの切り替えを追加しました。
- Copilot agents ページを Mission control として使い、アクセシビリティ セッションを監視して調整しました。
- cloud agent の pull request をレビューし、`@copilot` に変更を依頼して、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。さらに学びたい場合は、ほかのハーネスで今回の内容を発展させられます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat の agent mode と MCP 統合を体験します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot、agent skill、カスタム QA agent、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使い、ターミナルからレビュー済みの変更を 2 件リリースします。

自分のリポジトリでは、次の内容を試してみましょう。

- リファクタリングやテスト カバレッジの issue を cloud agent に割り当てる。
- 別の分野向けのカスタム エージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: Agent の管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
