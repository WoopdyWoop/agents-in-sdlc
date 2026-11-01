---
title: "レッスン 5 - quality-checks skill をカスタマイズして使う"
description: "既存の quality-checks skill を確認してレポート形式をカスタマイズし、フィルタリングを検証するために実行します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、必要な作業があります。これまでにコードが動作することを手動で確認し、instruction file を使って標準に従うよう Copilot を導きました。テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このような作業には、**agent skill** が適しています。skill は、こうした操作を適切に実行する方法を Copilot に伝えます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` skill を確認します。
- 結果のレポート形式をカスタマイズします。
- skill を再読み込みして実行します。

## シナリオ

Tailspin Toys は、ユニット テスト、lint、型チェックに `quality-checks` skill を使っています。チームでは、結果を読みやすくするため、レポートを改善したいと考えています。

## Instruction、script、resource

Agent skill には、再利用可能なタスクの instruction、実行可能な script、補助的な resource がまとまっています。agent は必要に応じてこれらを読み込みます。基本的には、skill 名のフォルダーと、その中にある `SKILL.md` で構成されます。Markdown ファイルには skill の名前と説明を定義する frontmatter、skill の概要、呼び出すタイミングのガイダンスを記述します。フォルダーには、呼び出されたときに使用する script やほかの resource を格納するサブフォルダーも追加できます。

> [!NOTE]
> skill に追加ファイルやフォルダーを含める必要はありません。Tailspin Toys の `quality-checks` skill は、プロジェクトにある既存の command を利用するため、`SKILL.md` だけで構成されています。

skill は、チームで共有して再利用するためにプロジェクトの `.github/skills` に置くことも、ユーザーの skill フォルダー `~/.copilot/skills` に置くこともできます。

## Skill を確認する

Tailspin Toys のチームがユニット テスト、lint、型チェックに使う `quality-checks` skill を確認しましょう。

1. Codespace に戻り、Codespaces のエディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。description は Copilot が skill を呼び出すタイミングの判断に使います。
3. instruction を読み、テストと lint の手順を Copilot にどのように案内しているか確認します。
4. **Results output formatting** セクションがまだないことを確認します。

## 変更前に skill を実行する

Copilot CLI から直接 skill を呼び出したり、自然言語で実行を依頼したりできます。Copilot に 3 種類のチェックを実行してもらいましょう。

1. Interactive モードで、フィルタリングの会話に戻ります。
2. 次のプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

何が実行され、成功したか、ツールが実際に何を報告したかが分かるレポートにしましょう。skill を更新して、その形式で報告するようにします。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新した skill を実行する

変更を加えたので、Copilot CLI で動作を確認します。会話を再起動せずに、編集した skill を再読み込みできます。

1. 次の command を入力します。

   ```plaintext
   /skills reload
   ```

2. 前回とまったく同じプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されたレポートを確認し、最初の結果と比較します。

## まとめと次のステップ

既存の agent skill をカスタマイズして実行しました。このレッスンでは、次のことを行いました。

- ユニット テスト、lint、型チェック用の `quality-checks` skill を確認しました。
- 結果のレポート形式をカスタマイズしました。
- skill を再読み込みして実行しました。

フィルタリングの機能 PR に、この変更も含めます。次は、[Playwright MCP server][next-lesson] を使って Copilot にサイトを直接操作させます。

## ほかの skill の例

コミュニティの次の例は参考資料です。追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図と export script: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
