---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルタリング機能を調べます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスとの連携、Copilot が利用できる自動化機能の追加など、さまざまな作業があります。ここで役立つのが MCP server です。MCP server を使うと、CLI に組み込まれている機能に加えて、Copilot が使えるツールやサービスを拡張できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどう使うかを理解します。
- まだ利用できない場合は、Playwright MCP server を追加します。
- agent にブラウザーを操作させ、フィルタリング機能を調べます。

## シナリオ

ユニット テストとエンドツーエンド テストは重要ですが、UI の変更を検証するには実際に操作する必要があります。Copilot に利用者と同じように Web サイトを使わせることで、変更内容の自動確認を強化し、期待どおりに動作する確度を高めたいと考えています。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI agent が外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI agent は外部のツールやサービスとリアルタイムに通信できます。最新の情報にアクセスしたり、ユーザーに代わって操作を実行したりできます。

これらのツールやリソースには、AI agent と外部ツールやサービスをつなぐ MCP server を介してアクセスします。各 MCP server は、AI agent が利用できる異なるツールとリソースのセットを提供します。

よく使われている MCP server の例を紹介します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、pull request を管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright によるブラウザー自動化機能を提供します。

ほかにも多くの MCP server が利用できます。GitHub は、エコシステム内の発見やコントリビューションを促すため、[MCP registry][mcp-registry] を公開しています。

> [!CAUTION]
> MCP server は、プロジェクトのほかの依存関係と同様に扱ってください。使用する前にソース コードを確認し、公開元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP server を追加する

Playwright MCP server がまだ利用できない場合は追加し、Copilot が利用者と同じように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI ですでに Playwright MCP を設定している場合は、このセットアップ手順をスキップして次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP server の追加画面を開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押して server type が **STDIO** であることを確認し、続けて <kbd>Tab</kbd> を押します。
5. **Command** の入力画面に、次のコマンドを貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP server を保存します。
7. <kbd>Esc</kbd> を押して MCP の画面を終了します。

## Playwright で機能を確認するよう Copilot に依頼する

これまでは、機能が期待どおりに動くことを手動で確認しました。今度は Copilot で利用できる Playwright MCP server を使い、同じ確認を行います。

1. Copilot に Playwright MCP server を使って機能を検証するよう、次のプロンプトを送信します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は、Copilot が自動的に MCP server の使用方法を判断するため、明示的に指定する必要はありません。ただし、使用するツールが分かっている場合は、プロンプトで案内すると結果の一貫性が高まり、token の節約にもつながります。

2. Copilot がブラウザーでどのような手順を実行し、機能を確認するか見てみましょう。
3. 報告を読み、すべてが期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作して、サーバーを停止したうえで結果を報告します。

## まとめと次のステップ

Playwright MCP server を使い、Copilot CLI から実際のブラウザーで機能を確認できました。要点は次のとおりです。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどう使うかを学びました。
- まだ利用できない場合は、Playwright MCP server を追加しました。
- agent にブラウザーを操作させ、フィルタリング機能を調べました。

次は、skill とブラウザー ツールを組み合わせた[QA カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP server を追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
