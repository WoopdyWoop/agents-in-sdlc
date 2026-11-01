---
title: "演習 5 - Agent の監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム エージェント][previous-lesson] |
|:--|

GitHub Copilot の agent mode では、コードベースの調査、変更の提案、ファイルの編集、コマンドの実行などを自律的に行います。VS Code の Copilot Chat では、ツールの呼び出しやファイルの編集がリアルタイムに表示されます。エディターを離れずに差分を確認し、個々の変更を採用または取り消し、追加のプロンプトで作業を調整・拡張できます。

この演習では、次のことを行います。

- アクセシビリティ agent の作業をリアルタイムに監視します。
- 提案されたファイル変更をレビューして採用します。
- セッションの途中で agent に指示し、ハイ コントラストに加えてライト モードを追加します。

## シナリオ

[演習 4][previous-lesson]で使ったアクセシビリティ用カスタム エージェントが、ハイ コントラスト モードを実装しています。作業中にデザイン チームから、既定のテーマが暗すぎると感じる利用者向けに、ライト モードも追加してほしいと依頼がありました。新しい会話を最初から始めずに、既存のセッションを調整して作業を追加します。

## Copilot の進捗を監視する

Copilot Chat ビューには、agent が調査するファイル、実行するコマンド、提案する編集など、各ステップが表示されます。ファイルの編集は差分と **Keep** / **Undo** の操作とともに表示されます。

1. 演習 4 で使った Copilot Chat ビューに戻ります。
2. agent の作業中に chat をスクロールし、各ツールの呼び出しを確認します。ファイルの読み取りや編集、実行を求められたターミナル コマンドが表示されます。
3. agent がファイルの編集を提案すると、差分が chat に表示されます。
   - 変更を採用するには **Keep** を選択します。
   - 変更を取り消して agent に再度作業させるには **Undo** を選択します。
4. chat の上部にある **Files changed** を開くと、このセッション中に agent が変更したファイルをすべて確認できます。

> [!NOTE]
> ターミナル コマンドの実行などに許可が必要な場合、agent は一時停止して **Allow** を表示します。内容を確認し、許可してもよいコマンドだけを承認してください。

5. agent がハイ コントラスト モードの実装を終えるまで待ちます。最後に変更の概要が表示されます。

## ライト モードを追加するよう agent に指示する

ハイ コントラスト モードができたので、同じ会話でライト モードの切り替えを追加します。同じ chat セッションを続けるため、agent は前の作業で触れた component、スタイル、local storage のパターンなどを把握しています。

1. Copilot Chat の入力欄に、次の追加プロンプトを送信します。

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. agent は前の変更を確認し、ライト モードの追加を提案します。
3. 追加された編集を確認し、差分ごとに **Keep** または **Undo** を選択します。
4. agent が望ましくない方向に進んだ場合（たとえば、切り替え UI を意図しない形で再構成しようとした場合）は、chat 入力欄の隣にある **Stop** を選択し、追加のメッセージで意図を説明します。

> [!TIP]
> 進行中のセッションを調整する方が、最初から始めるより早く済みます。追加プロンプトで変更を依頼できます（「CSS を簡潔にして、selector の重複ではなく CSS 変数を使う」など）。方向を修正したり（「3 つ目の切り替えを追加せず、ライト モードを既存の設定パネルに追加する」など）、範囲を広げたり（「両方の切り替えにキーボード ショートカットも追加する」など）できます。

## 変更内容をレビューする

作業を commit する前に、agent が変更した内容を確認します。

1. VS Code で **Source Control** ビューを開きます。
2. 変更されたファイルをすべて確認します。Astro component、スタイル、関連テストなどが更新されているはずです。
3. 変更されたファイルをいくつか開き、差分を確認します。カスタム エージェントのアクセシビリティ パターン（ARIA 属性、キーボード操作、セマンティック HTML、local storage による設定の保持）が反映されていることを確認します。
4. 問題がなければ **Source Control** パネルから変更を stage して commit します。次のレッスンで公開します。

## まとめと次のステップ

Copilot Chat の agent mode で進行中のセッションを監視し、各ファイル編集を確認し、変更を採用または却下し、コンテキストを保ったまま追加プロンプトを送って作業を拡張しました。

このレッスンでは、次のことを行いました。

- agent の作業をリアルタイムに監視しました。
- 個々のファイル編集を採用または却下しました。
- 追加プロンプトでセッションの途中に指示を出しました。

次のレッスンでは、[アクセシビリティの変更を pull request として公開し][next-lesson]、このラボでローカルのカスタム エージェントが作成した変更をレビューします。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [Agent mode を使う][agent-mode]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
