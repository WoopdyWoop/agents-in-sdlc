---
title: "レッスン 7 - QA agent を作成して使う"
description: "issue の要件、quality-checks skill、Playwright MCP を組み合わせる QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに `quality-checks` skill で自動チェックを実行し、Playwright MCP でブラウザーからフィルタリング機能を確認しました。次は、明確な QA プロセスを備えたカスタム エージェントを作り、これらの機能をまとめます。

このレッスンでは、次のことを行います。

- カスタム エージェントが instruction、skill、MCP ツールと連携する仕組みを確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA agent を選択し、フィルタリングの issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys では、pull request (PR) を作成する前に、要件、コード品質、自動チェック、テスト範囲、ブラウザー上の動作を一貫して確認したいと考えています。カスタム エージェントを使うと、QA の手順をまとめて実行し、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の特化版です。プロファイルには、agent の目的、instruction、使用できるツールを記述します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの instruction はチーム標準を記述し、`quality-checks` skill は繰り返し実行できるチェックをまとめ、Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、それらを使って要件を評価し、結果を報告する方法を Copilot に伝えます。これらを置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、Copilot に再利用可能な QA プロファイルを作成してもらいます。プロファイルには QA が行うチェックと、その作業の境界を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定の agent に、次のカスタム エージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しい agent を使う前に、Copilot が想定した QA の流れと制約を反映したか確認します。確認だけが目的なのに、プロファイルが不完全だったり範囲が広すぎたりして、機能を変更することを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は任意ですが、指定すると agent の表示名が分かりやすくなります。
3. プロファイルの instruction を読み、QA が要件を確認し、リポジトリの instruction に従い、`quality-checks` skill を実行して、Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に確認を求め、変更を commit せず、pull request も開かないことを確認します。
5. これらの役割や制約が不足していたら、続行する前に既定の agent へ修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は、会話の開始時にプロジェクトの agent を読み込みます。プロファイルを作成したら同じフィルタリングの会話を再開し、QA を選択して、会話にある issue と計画の内容を使わせます。

1. 次のプロンプトを入力して agent を有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> agent を作成した直後は、利用可能な agent の一覧に表示されない場合があります。その場合でも、上記のコマンドで有効にできます。

2. QA agent に機能をレビューしてもらうため、次のプロンプトを送信します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA agent が作業を進めます。
4. 完了したら、レポートを確認します。

## まとめと次のステップ

再利用可能な専門的役割を追加し、その作業をレビューしました。このレッスンでは、次のことを行いました。

- カスタム エージェントが instruction、skill、MCP ツールと連携する仕組みを確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA agent を選択し、フィルタリングの issue に照らして結果をレビューしました。

実装、skill の更新、QA プロファイル、テスト、検証レポートの準備ができました。次は、[それらを機能の PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
