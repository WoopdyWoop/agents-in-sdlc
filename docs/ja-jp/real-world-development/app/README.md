---
slug: ja-jp/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** は Copilot CLI を基盤とするデスクトップ アプリケーションで、agent 主導の開発を1つの専用 workspace で実現します。並列 agent セッション、切り替え可能なセッション モード、共有 canvas、GitHub issue と pull request のネイティブな管理機能を備えています。さらに **Agent Merge** を使うと、rebase、レビュー フィードバック、継続的インテグレーション (CI) の修正、マージを通じて pull request の処理を進められます。

このワークショップでは、Tailspin Toys の一連のワークフローに沿って作業します。

1. プロジェクトを準備し、app をインストールし、リポジトリを接続して、workspace と用意されたバックログを確認します。
2. 星評価の小さな変更を加え、ブラウザーでレビューして、最初の pull request (PR) を手動でマージします。
3. フィルタリングの issue から始めて、**Plan** モードでアプローチを決め、**Autopilot** モードで実装し、**Interactive** モードでレビューします。
4. リポジトリの instruction を更新し、フィルタリングの作業に適用します。
5. 既存の `quality-checks` skill をカスタマイズして、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) server を追加し、ブラウザーでフィルタリング機能を確認します。
7. 品質保証 (QA) 用のカスタム エージェントを作成し、要件、カバレッジ、検証の根拠をレビューします。
8. フィルタリングの変更全体をレビューし、2 件目の PR に Agent Merge を使います。
9. canvas が利用できるか確認してから、Database Explorer を使い、リポジトリに保存するトリアージ canvas を作成してテストします。必要な機能を利用できない場合は、ブロッカーを記録し、読み取り専用の代替手段を使うか、まとめに進みます。

ワークショップの焦点を絞るため、PR を 2 件作成します。1 件目は星評価、2 件目はフィルタリングと、instruction の更新、skill の更新、QA profile、テストです。それぞれ更新済みの `main` から開始します。フィルタリングと品質ワークフローでは、1 つのセッション、worktree、ブランチを共有するため、各ツールを学びながら作業を積み重ねられます。最後の canvas 演習も同じセッションで進め、PR ワークフローを繰り返すのではなく、共有画面の作成とテストに集中します。

## レッスン

| レッスン | トピック | 説明 |
|--------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | Node.js をインストールし、Tailspin Toys プロジェクトの自分用コピーを作成します |
| [1. Copilot app のインストール][ex1] | セットアップ | app をインストールしてプロジェクトを接続し、workspace を確認します |
| [2. 星評価を追加する: 小さな改善][ex2] | 最初の変更 | 既存の評価と評価未設定時の表示を追加し、最初の PR をマージします |
| [3. Agent モード: Plan と Autopilot][ex3] | Agent モード | issue から機能を計画し、Autopilot で実装して、Interactive でレビューします |
| [4. カスタム instruction で Copilot を導く][ex4] | コンテキスト | instruction を確認・更新して、フィルタリングに適用します |
| [5. quality-checks skill をカスタマイズして使う][ex5] | 繰り返し可能なチェック | 既存の skill を確認してレポート形式を変更し、実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーによる確認 | Customize から MCP を設定し、フィルタリングの動作を調べます |
| [7. QA agent を作成して使う][ex7] | 要件とカバレッジ | 専門 profile を作成して選択し、最終的な検証の根拠を集めます |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | フィルタリング、instruction、skill、QA profile、テストをレビューし、2 件目の PR に Agent Merge を使います |
| [9. Canvas を確認して作成する][ex9] | コラボレーション | Database Explorer を使い、リポジトリに保存するトリアージ canvas を作成してテストします |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、成果物、さらに学ぶためのリソースを確認します |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランを持つ GitHub アカウント
- [ ] **macOS、Linux、Windows** を実行するコンピューター
- [ ] コンピューターに[インストールした Git][install-git]

> [!TIP]
> 有料プランを利用できない場合、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、コードレビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

> [!NOTE]
> Copilot app は Codespace ではなく自分のコンピューターで動作します。[前提条件のレッスン][ex0]では、app をインストールする前に Node.js をインストールし、プロジェクトのコピーを作成します。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合、管理者が **GitHub Copilot app** のポリシーを有効にしておく必要があります。この設定は Copilot CLI のポリシーとは別です。

## 開始する

**[前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
