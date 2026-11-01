---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、pull request の自動化を学びながら、Tailspin Toys に 2 つの変更を実装、検証、リリースします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** を使うと、ターミナルで GitHub Copilot を agent 型のコーディング アシスタントとして利用できます。コードベースを調べ、コードを生成し、コマンドを実行し、外部ツールに接続できます。グラフィカル エディターに切り替えずに、コマンド ラインから作業を続けられます。

このワークショップでは、Tailspin Toys の一連のワークフローに沿って作業します。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして、使い方を確認します。
2. 星評価の小さな変更を加え、ブラウザーでレビューして、最初の pull request (PR) を手動でマージします。
3. フィルタリングの issue から始めて、Plan モードでアプローチを決め、Autopilot モードで実装し、Interactive モードでレビューします。
4. リポジトリの instruction を更新し、フィルタリングの作業に適用します。
5. 既存の `quality-checks` skill をカスタマイズして、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) server を追加し、ブラウザーでフィルタリングを確認します。
7. 品質保証 (QA) 用のカスタム エージェントを作成し、要件、テスト範囲、検証の根拠をレビューします。
8. フィルタリングの変更全体をレビューし、Agent Merge を使って PR をマージします。
9. コンテキスト、モデル、共有、cloud agent への任意の委任に使う slash command を確認します。

ワークショップを簡潔にするため、星評価と、instruction の更新、skill の更新、QA profile、テストを含むフィルタリングの 2 件の PR を作成します。各ツールの使い方を確認しながら作業を続けられるよう、フィルタリングと品質ワークフローでは 1 つの会話とブランチを使います。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、基本を確認する |
| [2. 星評価を追加する: 小さな改善][ex2] | 最初の変更 | 既存の評価と評価未設定時の表示を追加し、最初の PR をマージする |
| [3. Agent モード: Plan と Autopilot][ex3] | Agent モード | issue から機能を計画し、Autopilot で実装して、Interactive でレビューする |
| [4. カスタム instruction で Copilot を導く][ex4] | コンテキスト | instruction を確認・更新して、フィルタリングに適用する |
| [5. quality-checks skill をカスタマイズして使う][ex5] | 繰り返し可能なチェック | 既存の skill を確認してレポート形式を変更し、実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーによる確認 | CLI で MCP を設定し、フィルタリングの動作を調べる |
| [7. QA agent を作成して使う][ex7] | 要件とテスト範囲 | 専門 profile を作成して選択し、最終的な検証の根拠を集める |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使う |
| [9. GitHub Copilot CLI のスラッシュ コマンド][ex9] | CLI の機能 | コンテキスト、モデル、共有、cloud agent への任意の委任を確認する |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、再利用可能なカスタマイズ、参考資料を振り返る |
| [オプション: Foundry を組み込む][foundry] | ホスト型 agent | モデルを準備し、カタログに基づく agent をデプロイして Web サイトに接続する |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] ターミナル / コマンド ライン操作の基本的な知識
- [ ] インストール済みで設定済みの Git

> [!TIP]
> 有料プランを利用できない場合、認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、コードレビュー、Copilot CLI の機能が含まれています。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[foundry]: 8-foundry-agent/
[ex10]: 10-review/
