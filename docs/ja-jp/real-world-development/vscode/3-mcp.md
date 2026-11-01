---
title: "演習 3 - Playwright MCP server で機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

agent mode でフィルタリング機能を作成しました。pull request を開く前に、ブラウザーで動作を確認します。自分でアプリを操作する代わりに、**Playwright MCP server** を接続し、Copilot に実際のブラウザーで機能をテストしてもらいます。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP server が Copilot にツールを追加する仕組みを学びます。
- Playwright MCP server を workspace に追加します。
- ブラウザーでフィルタリング機能を手動テストするよう Copilot に依頼します。
- ブランチを公開し、フィルタリングの pull request を作成します。

## Model Context Protocol (MCP) とは

agent mode は、エディターの外部にあるツールを利用できると、さらに便利になります。Copilot がそのために使うのが Model Context Protocol (MCP) です。agent が外部ツールやサービスと通信するための標準的な方法です。

![agent mode の内部構造と、コンテキスト、LLM、MCP server と VS Code 拡張機能が提供するツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI agent が外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI agent は外部のツールやサービスとリアルタイムに通信できます。最新情報（resource）にアクセスしたり、ユーザーに代わって操作（tool）を実行したりできます。

これらのツールやリソースには、AI agent と外部ツールやサービスをつなぐ MCP server を介してアクセスします。MCP server は、既存の API や NPM package のようなローカル ツールと AI agent との通信を管理します。各 server は、AI agent が利用できる異なるツールとリソースのセットを提供します。

よく使われている MCP server の例を紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。新しいリポジトリの作成、既存リポジトリの更新、issue や pull request の管理などを agent が実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などを agent が実行できます。

ほかにも多くの MCP server があり、さまざまなツールやリソースを利用できます。GitHub は、エコシステム内での発見やコントリビューションを促すため、[MCP registry](https://github.com/mcp) を公開しています。

> [!CAUTION]
> セキュリティの観点から、MCP server はプロジェクト内のほかの依存関係と同様に扱ってください。使用前にソース コードを慎重に確認し、公開元を検証して、セキュリティへの影響を検討してください。信頼できる MCP server のみを使用し、機密性の高いリソースや操作へのアクセスを許可する際は慎重に判断してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code workspace から利用できる MCP server を設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次のように、`github` server がすでに設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` の設定はプロジェクト テンプレートに含まれています。そのため、前の演習で Copilot がバックログの issue を読み取れました。この設定は[リモート GitHub MCP server][remote-github-mcp-server]を使います。ローカルにインストールする必要はなく、VS Code が HTTP で接続します。Copilot が初めて tool を使うときに GitHub で認証します。

## Playwright MCP server を追加する

次に、2 つ目の server を追加します。[Playwright MCP server][playwright-mcp-server] は、Copilot が操作できるブラウザーを提供するため、機能のテストに役立ちます。

1. `.vscode/mcp.json` で `github` と並べて `playwright` を追加し、ファイルが次のようになるようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` server とは異なり、`playwright` は**ローカル** server です。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` flag は Playwright に、画面を表示せずにブラウザーを実行するよう指示します。デスクトップ画面がない Codespace ではこの設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンド テストにすでに Playwright を使っているため、必要なブラウザーもインストール済みであることがほとんどです。Copilot が後でブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行してから再試行してください。

## Server を起動して信頼する

VS Code は必要に応じて MCP server を起動します。Copilot が初めてその server のツールを必要としたときに、VS Code は server を起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` 設定の上に **Start** action が表示されます。選択して server を起動し、設定が正しいことを確認します。
2. VS Code で server を信頼するか確認されたら、設定を確認して、server を起動できるよう信頼することを選択します。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot が初めてツールを必要としたときに VS Code が起動し、信頼を確認します。

## フィルタリング機能をテストする

[Playwright MCP server][playwright-mcp-server] は、Copilot に実際のブラウザーを操作させます。自分でアプリを操作しなくても、agent がページを開き、移動し、filter を適用し、結果を読み取って要約します。会話を離れずに機能を確認できる手早い方法です。

内部では、Playwright MCP server はスクリーンショットではなく、ページの [accessibility tree][playwright-mcp-server] を使います。そのため、agent はボタン、リンク、リスト項目など、ラベル付きの構造化された要素を支援技術と同じように扱います。簡単な機能確認が、アクセシビリティの軽い健全性チェックにもなります。

server を接続し、アプリを起動したら、次のプロンプトを Copilot に送信して、作成したフィルタリング機能を確認します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP server 経由でブラウザーを起動し、各ステップを実行して結果を報告します。issue の受け入れ条件に照らして報告を読みます。問題がある場合は、追加の質問をするか、pull request を開く前にコードの修正を依頼します。

> [!NOTE]
> このテストには `http://localhost:4321` で実行中の app が必要です。dev server を停止している場合は、プロンプトを送る前に再起動します。Copilot が初めて Playwright MCP server を使う際、ブラウザーをダウンロードする場合があります。見つからないと報告されたら、`npx playwright install chromium` を実行して再試行してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開して pull request を作成する

機能が動作することを確認できたら、チームのレビュー用に pull request (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace で **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更を stage します。
3. **Sparkle** ボタンを使って commit message を生成します。

   ![変更内容を示す Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリに push します。

## Pull request を作成する

github.com や GitHub command-line interface (CLI) など、pull request を作成する方法はいくつかあります。GitHub Copilot を使っているので、PR の作成も依頼しましょう。関連する issue を見つけ、紐づけた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. agent のドロップダウンで **Agent** が選択されていることを確認します。Copilot が GitHub のツールを使えるようにします。
3. 次のプロンプトを送信して PR を作成します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を収集して操作を行うために **Continue** を求めたら選択します。GitHub tool を初めて使うときに認証を求められた場合は、案内に従います。
5. Copilot が issue を検索して該当するものを見つけ、PR を作成したことを確認します。
6. Copilot が作成したリンクを選択して PR をレビューします。ただし、まだ**マージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP server が Copilot にツールを追加する仕組みを学びました。
- Playwright MCP server を workspace に追加しました。
- 出荷前に、ブラウザーでフィルタリング機能を手動テストしました。
- ブランチを公開し、フィルタリングの pull request を作成しました。

MCP server を使って機能を*テスト*しました。MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティレビューなど、対象を絞ったタスクを効率化する[カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP server][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
