---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を体験します。変更の計画、コードの作成、動作のテスト、pull request のレビュー、agent を使った作業管理を反復して行います。

## シナリオ

架空の会社 Tailspin Toys に、新しい開発者として参加します。同社は、開発者をテーマにしたボード ゲームのクラウドファンディングを提供しています。チームのバックログは GitHub issue として作成済みなので、すぐに着手できます。フィルタリングやページネーションなどの機能開発に加え、アクセシビリティやコーディング標準の改善も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、さまざまな作業環境で利用できます。希望する開発方法に合った環境を選択してください。各ワークショップは、それぞれ独自のセットアップから始まり、共通の Tailspin Toys シナリオを使います。

### [GitHub Copilot CLI][cli]

ターミナルで agent 型のアシスタントを使います。Plan と Autopilot の各モード、instruction、skill、カスタム エージェント、Playwright MCP、Agent Merge、便利な slash command を扱います。

### [GitHub Copilot app][app]

デスクトップ app で agent セッションを並行して実行し、セッション モードを切り替え、Canvas で共同作業を行い、GitHub issue と pull request を管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub issue を非同期 agent に割り当て、作業を案内して進捗を確認し、agent が作成した pull request をレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat の agent mode、MCP server、カスタム エージェントを利用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
