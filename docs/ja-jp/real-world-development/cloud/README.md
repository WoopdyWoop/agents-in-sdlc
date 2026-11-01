---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウドで非同期に作業できます。GitHub で作業を割り当てると、cloud agent はバックグラウンドでリポジトリを調べ、変更を加え、pull request を作成します。その間、別の作業を続けられます。

この演習では、まず cloud agent が従うカスタム instruction を追加し、GitHub issue を割り当てて実装を任せます。カスタム エージェントを使って方針を定め、agent ダッシュボードからセッションの進捗を確認・調整し、最後に pull request をレビューして結果を反復改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. カスタム instruction][ex1] | コンテキスト | cloud agent が従うカスタム instruction を追加する |
| [2. Cloud agent][ex2] | 非同期 agent | GitHub issue を Copilot cloud agent に割り当てる |
| [3. カスタム エージェント][ex3] | 専門 agent | カスタム エージェントを確認して使う |
| [4. Agent の管理][ex4] | 監視 | agent セッションを監視して調整する |
| [5. 反復改善][ex5] | レビュー | PR をレビューし、Copilot の作業を反復改善して、次の手順を選ぶ |

## 前提条件

このワークショップに参加する前に、次の条件を満たしていることを確認してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランを持つ GitHub アカウント
- [ ] アカウントまたは組織で Copilot cloud agent が有効になっている

> [!TIP]
> 有料プランを利用できない場合、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、コードレビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent は **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free を除くすべてのプランが対象です。Copilot Business または Enterprise では、管理者が組織で有効にする必要があります。

> [!NOTE]
> このハーネスでは MCP を扱いません。Copilot で MCP server を使う方法については、[CLI ハーネス](../cli/) または [VS Code ハーネス](../vscode/)を参照してください。

## 開始する

**[演習 0: 前提条件から開始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
