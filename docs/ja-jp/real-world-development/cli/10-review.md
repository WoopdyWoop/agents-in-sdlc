---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 件の pull request、再利用可能なカスタマイズ、さらに学ぶためのリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローに取り組みました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、作成済みのフィルタリング issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初の pull request (PR) を手動でマージしました。
- フィルタリングの issue から始めて、Plan モードでアプローチを決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム instruction で agent を導き、既存の `quality-checks` skill をカスタマイズし、それを使ってユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) server を追加し、実際のブラウザーでフィルタリング機能を確認しました。
- 品質保証 (QA) 用のカスタム エージェントを作成して選択し、要件、テスト範囲、skill の結果、ブラウザーでの根拠を評価しました。
- フィルタリングの変更全体をレビューし、Agent Merge によるフィルタリング PR のマージを承認しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使う slash command を確認しました。

## リリースした内容

ワークショップでは、更新済みの `main` からブランチを作成し、次の 2 件の PR を扱いました。

1. **星評価:** 既存の `starRating` と、評価が未設定であることを示す表示をゲーム カードに追加します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、instruction を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA profile を作成して、関連するテストを追加します。

フィルタリングの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を介した Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` skill はユニット テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA agent は PR の前に、要件とテスト範囲のレビューにこれらの結果を組み合わせました。

追加するテストは、実際の不足を解決する必要があります。QA の実行で追加テストが不要と判断される場合もあります。マージを承認する前にコードと根拠をレビューし、変更後は関連する根拠を再確認してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールは、作業内容に影響します。このワークショップでは、instruction の更新、skill のカスタマイズ、QA profile の作成、MCP server の設定を行いました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。instruction は標準を設定し、skill は繰り返し実行できるタスクを記述し、カスタム エージェントは専門的な役割を定義し、MCP server は外部ツールに接続します。agent の要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前に方針を検討するとき、**Interactive** は範囲を絞って確認しながら進めるとき、**Autopilot** は範囲の明確なタスクに使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選択します。

環境と同様に、コンテキストも重要です。*何を*作りたいか、*なぜ*必要か、*どのように*進めたいかを明確に伝えると、出力が大きく変わります。

## さらに確認する

中心的なワークフローは完了しました。Copilot CLI の機能をさらに確認する場合は、次の slash command を使ってみましょう。

- `/review` で code review agent に変更を分析してもらいます。
- `/rubber-duck` で問題を整理し、別の視点から検討します。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` でタスクを Copilot cloud agent に送信します。

## 次のステップ

どのツールでも、上達する最善の方法は使い続けることです。本番コード、趣味のコード、長年作りたいと思っていた小さなアプリなどで活用してください。チームと学びを共有し、互いに学びましょう。ドキュメントも確認してください。

ターミナルから Tailspin Toys をさらに拡張する場合は、オプションの[Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境も体験する場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI のコマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI で pull request を管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
