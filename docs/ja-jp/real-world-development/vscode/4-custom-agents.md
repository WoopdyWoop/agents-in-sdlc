---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや領域に合わせて、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルで agent を定義すると、目的を絞った instruction、ベスト プラクティス、コーディング パターン、ドメイン固有の知識を Copilot に渡し、特定の作業を効果的に進められます。チームの専門知識を再利用可能な agent にすることもできます。たとえば、[WCAG][wcag] 準拠を徹底するアクセシビリティ agent、安全なコーディング プラクティスに従うセキュリティ agent、一貫したテスト パターンを守るテスト agent などです。

カスタム エージェントは、プロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、その後に agent の振る舞い、専門性、instruction を定義する Markdown prompt が必要です。

### カスタム エージェントと agent skill の比較

カスタム エージェントと [agent skill][agent-skills-concept] には、概念的に重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。分かりやすく言うと、**カスタム エージェント**は作業者で、**skill** はツールです。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、skill やほかの agent を連携させながら作業するよう設計されています。このラボでは、アクセシビリティ用のカスタム エージェントがガイドラインに沿ってサイトをレビューし、更新します。その過程で、pull request ワークフロー用の skill やテストを実行・管理する skill などを呼び出す場合があります。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI のほかの用途と同じように、環境やシナリオに合う方法をテストし、反復して見つけましょう。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のフィードバックでは、現在のダーク テーマはテキストと背景色のコントラストが不十分で、読みにくいと指摘されています。この問題に対応するため、デザイン チームは、ユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装します。機能の生成にはカスタム エージェントを使います。この演習では、次のことを行います。

- 既存のアクセシビリティ用カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ agent を使って、ハイ コントラスト モードを実装します。

## アクセシビリティ用カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。Copilot をどのように導くか確認しましょう。

Codespace に戻り、ターミナルを開いて、アクセシビリティの作業用に `main` から新しいブランチを作成します。Exercise 3 のフィルタリング PR とはブランチを分けます。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主な責務。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある落とし穴や間違い。

## Copilot Chat でカスタム エージェントを使う

VS Code の Copilot Chat ビューの下部にある agent のドロップダウンには、`.github/agents` に定義したすべてのカスタム エージェントが表示されます。カスタム エージェントを選択すると、その agent 専用の instruction とツールを使う chat セッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

1. Chat ビューの agent ドロップダウンで、まだ選ばれていない場合は **Agent** を選択します。

   ![Chat ビューの agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. Chat ビューの下部にある agent ドロップダウンを選択します。既定では、現在の agent 名として **default** が表示されています。
3. 利用可能な agent の一覧から **Accessibility agent** を選択します。
4. アクセシビリティ agent に次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べ、変更を提案して、プロジェクト ファイルを編集します。編集内容は、ファイル パスと差分とともに chat に表示されます。
6. agent が編集したファイルの一覧は、**Files changed** の表示で確認できます。

> [!NOTE]
> この処理には数分かかります。Copilot は Astro component、CSS、関連するテストなどの既存ファイルを編集します。

次の演習では、作業中の agent を確認し、指示を追加します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]について学びました。カスタム エージェントは、特定のタスクや領域に合わせた専門的な AI アシスタントです。チームの専門知識や標準を再利用可能な agent にし、Copilot が作業を効果的に進められるように導けます。

このレッスンでは、次のことを確認しました。

- カスタム エージェントの定義方法。
- Copilot Chat の agent mode でカスタム エージェントを使う方法。

次は、[agent の作業を監視して調整します][next-lesson]。同じセッションで変更をレビューし、ライト モードの切り替えを追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP で機能をテストする][previous-lesson] | [次のレッスン: Agent の監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
