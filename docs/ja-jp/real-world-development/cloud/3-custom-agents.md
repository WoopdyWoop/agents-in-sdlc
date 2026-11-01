---
title: "演習 3 - カスタム エージェント"
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

カスタム エージェントを使って、次のことを学びます。

- カスタム エージェントの定義方法。
- カスタム エージェントへのタスクの割り当て方。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のフィードバックでは、現在のダーク テーマはテキストと背景色のコントラストが不十分で、読みにくいと指摘されています。この問題に対応するため、デザイン チームは、ユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装します。機能の生成にはカスタム エージェントを使います。

## アクセシビリティ用カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。Copilot をどのように導くか確認しましょう。

1. Codespace に戻り、アクセシビリティ用カスタム エージェントのファイルを確認します。
2. `.github/agents/accessibility.md` を開きます。
3. `name` と `description` フィールドがある YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` を含む frontmatter が必要です。

4. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主な責務。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある落とし穴や間違い。

## issue を作成して割り当てる

Mission control は、環境内の agent を管理する中心的な場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、追加のガイダンスを与えられます。まず、ハイ コントラスト モードの作成タスクを Copilot に割り当てます。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue の画面を開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当て画面を開きます。
9. カスタム エージェントの一覧から **Accessibility agent** を選択します。

    ![カスタム エージェントの割り当て画面で、Accessibility agent が選択されているスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot はバックグラウンドで作業を開始します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]について学びました。カスタム エージェントは、特定のタスクや領域に合わせた専門的な AI アシスタントです。チームの専門知識や標準を再利用可能な agent にし、Copilot が作業を効果的に進められるように導けます。

このレッスンでは、次のことを学びました。

- カスタム エージェントの定義方法。
- カスタム エージェントへのタスクの割り当て方。

Copilot がハイ コントラスト モードを実装する間に、[Mission control から agent セッションを監視して調整します][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使う準備][org-custom-agents]
- [Enterprise でカスタム エージェントを使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: Agent の監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
