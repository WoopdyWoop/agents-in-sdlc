---
title: "演習 4 - Agent の監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習では、ユーザー体験の改善と機能追加を目的に、Copilot cloud agent に 3 つの別々のタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

GitHub.com の[agents ページ][agents-page]など、cloud agent に割り当てたタスクを管理する方法はいくつかあります。この Mission control では、pull request (PR) が開かれているすべての agent タスクを確認できます。処理内容を調べたり、進行中のセッションを調整したりできます。

このレッスンでは、次のことを行います。

- agents ページで cloud agent のタスクを監視します。
- 進行中のセッションを調整し、機能追加を依頼します。

## シナリオ

ハイ コントラスト モードを作成するよう agent に割り当てたあと、チームはライト モードも追加するよい機会だと考えました。サイトのスタイルを更新し、切り替え機能を実装しているところなので、ライト モードも含めるのが自然です。ハイ コントラストに加えてライト モードも作成するよう、agent の作業を調整します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左ペインの両方に表示されるタスク一覧を確認します。次のようなタスクが表示されます。
   - コードのドキュメントを更新する。
   - ゲーム変更用の API を生成する。
   - Web サイトにハイ コントラスト モードを追加する。
3. 実行中のタスクを選択し、Copilot が行った処理を確認します。たとえば、リポジトリをチェックアウトする、Copilot の環境を作成する、MCP server をセットアップする、割り当てられたタスクを実行する、といった処理があります。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や実施したアプローチによって異なります。

4. 右側に表示される pull request (PR) ペインも確認します。ここから PR と変更ファイルを確認して、作業を監視できます。

## Cloud agent に追加の指示を出す

進行中のタスクを確認できたので、ハイ コントラスト モードの作業中にライト モードの切り替えも追加するよう Copilot に依頼します。

1. ハイ コントラスト モードを追加するセッションを選択します。タイトルは、Copilot が付けた名前や作業状態によって異なります。

    ![Mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まるまで、数分間セッションを確認します。次のようなメッセージが表示されたら作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot の作業中にセッションを調整するダイアログを示す agents ページのスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に追加することを確認します。

## Copilot に作業を任せる

これまでと同様に、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業していたステップが終わると、新しい依頼が作業に反映されます。

処理には数分かかります。その間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot agents ページについて学びました。ここは GitHub Copilot cloud agent のタスクを監視し、作業を案内する中心的な場所です。進行中と完了済みのタスクを追跡し、実行内容を確認して、進行中のタスクの範囲やガイダンスを変更できます。

このレッスンでは、次のことを行いました。

- Mission control と agents ページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加の機能を依頼しました。

Copilot がアクセシビリティ機能を完成させたら、[Copilot が作成した pull request を反復改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
