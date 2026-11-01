---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルタリング、instruction、skill の更新、QA profile、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、instruction の更新、skill の更新、品質保証 (QA) profile、テストは、すべて 1 つのブランチに保存されています。まとめてレビューし、pull request を作成しましょう。星評価の pull request (PR) は自分でマージしましたが、今回は**Agent Merge** にマージの手順を任せます。

> [!NOTE]
> 通常は機能、instruction の更新、skill の更新、QA agent をいくつかの PR に分けます。このワークショップでは手順を簡潔にするため、フィルタリングと品質ワークフロー全体を 1 つの会話とブランチにまとめ、1 つの PR に含めています。

このレッスンでは、次のことを行います。

- Agent Merge とは何か、マージまでの流れを自動化する方法を学びます。
- 機能全体の変更と検証の根拠を確認します。
- フィルタリングの PR を作成します。
- レビューした後にのみ Agent Merge を有効にし、PR のマージを確認します。

## シナリオ

フィルタリングのワークフロー全体で、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys は、マージの承認権限を開発者の管理下に置いたまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、pull request をマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR をブロックしている問題（CI チェックの失敗、レビュー コメントへの対応、必要な rebase など）に取り組みます。リポジトリで許可されている場合は、GitHub auto-merge も有効にします。

これまでは **Merge pull request** を自分で選択していました。Agent Merge にその手順を任せることもできます。Agent Merge を有効にする前に、変更をレビューして準備ができていることを確認してください。

## Agent Merge で PR を管理する

すべてのコードを作成できたら、変更をまとめてレビューして PR を作成し、残りの処理を Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力して agent ダイアログを開きます。
3. 選択肢から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視します。すべて成功すると、マージが実行されます。
7. `PR #14 was squash-merged successfully.` のような Copilot のメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されている場合は、レビュー済みの PR を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、pull request の処理など、開発プロセスの一部を自動化しました。具体的には、次のことを行いました。

- Agent Merge とは何か、マージまでの流れを自動化する方法を学びました。
- 機能全体の変更と検証の根拠を確認しました。
- フィルタリングの PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR のマージを確認しました。

次は、コンテキスト、モデル、共有、cloud agent への任意の委任に使う[便利な Copilot CLI の slash command][next-lesson]を確認します。

## リソース

- [Copilot CLI で pull request を管理する][manage-prs]
- [Copilot CLI のコマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
