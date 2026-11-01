---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキストの管理、モデルの選択、セッション共有、cloud agent への任意の委任に使う slash command を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

ほかの優れた CLI ツールと同様に、GitHub Copilot CLI にも多くの slash command があります。高度な機能や内部情報を確認したり、追加設定を行ったりできます。すでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` で、利用可能なモデルを確認します。
- `/share` で、セッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## シナリオ

CLI の主なワークフローが完了したので、コンテキストの管理、モデルの切り替え、セッション共有、[Copilot cloud agent][about-cloud-agent] への任意の作業委任など、追加機能を見ていきましょう。

## Copilot CLI のコンテキストを確認する

大きなタスクや複雑なタスクでは、モデルの最大コンテキスト ウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に compact します。また、slash command でコンテキストを確認したり、自分で compact したりできます。

1. Codespace に戻ります。Copilot CLI がまだ起動していない場合は、リポジトリ ルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在の token 使用量、システム instruction、ツール、メッセージ、空き領域に分割されたコンテキストを確認します。
4. 会話を compact します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話がすでに短い場合は、大きな変化がないこともあります。

> [!NOTE]
> Copilot CLI は、コンテキスト ウィンドウが埋まると自動的に compact します。実行するタイミングを選びたい場合は `/compact` を使います。関係のないタスクに切り替えるときに新しい会話を始めるには、`/clear` または `/new` を使います。

## モデルを選択する

モデルごとに得意分野が異なり、開発者の好みもさまざまです。Copilot CLI では、利用可能なモデルを一覧表示し、使うモデルを選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルをそのまま使うか、別のモデルを選択します。一覧を閉じるには <kbd>Esc</kbd> を押します。

## セッションを共有する

チームで協力して学びを共有すると、AI ツールの活用方法を改善できます。`/share` は、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みバージョンで利用できる `/share` のオプションを確認します。
2. セッションを共有する場合は、ローカルに Markdown を出力する `/share file` など、必要に合った出力先を選びます。
3. エクスポートした内容を、誰かに送信したり公開したりする前に確認します。セッションの出力には、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容を公開しないでください。

## 任意: 作業を拡張または委任する

ワークショップの中心的な内容は完了しました。大きなタスクでは、Copilot CLI の次のコマンドも利用できます。

- `/fleet` は、独立したサブタスクを subagent に割り当てて、並行して実行できます。
- `/worktree` は、別のタスク用に独立した Git worktree を作成できます。
- `/delegate` は、タスクを Copilot cloud agent に送信できます。cloud agent は非同期に作業し、pull request を開く場合があります。

これらのコマンドは、追加の worktree やリモート作業を作成するため、任意の内容です。試すときは、新しく範囲を明確にしたタスクから始め、通常のワークフローで結果をレビューしてください。非同期 agent 作業について詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI の slash command を使うと、設定を変更したり、セッションを共有したり、内部で起きていることを確認したりできます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` で、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` で、利用可能なモデルを確認しました。
- `/share` で、セッションをエクスポートまたは共有する方法を学びました。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

Copilot CLI には、ほかにも多くの slash command と機能があります。最後に[ここまでの学びを振り返り][next-lesson]、次の学習ステップを確認しましょう。

## リソース

- [Copilot CLI のコマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
