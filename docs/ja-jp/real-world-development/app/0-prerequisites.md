---
title: "レッスン 0 - 前提条件"
description: "GitHub Copilot app のレッスンに向けて、Tailspin Toys プロジェクト用の Node.js をインストールし、テンプレートからリポジトリの自分用コピーを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

GitHub Copilot app は Copilot と GitHub を一元的に扱うデスクトップ アプリケーションです。issue や pull request にすばやくアクセスでき、GitHub Copilot を使った開発もできます。このワークショップでは、自分のコンピューターで Astro 製の Tailspin Toys app を更新します。始める前に Node.js がローカルにインストールされていることを確認し、次に Copilot app をインストールしましょう。

このレッスンでは、次のことを行います。

- プロジェクトのテストをコンピューターで実行できるように Node.js をインストールします。
- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成します。

## Node.js をインストールする

いくつかのレッスンでは、agent に機能を実装させ、Tailspin Toys のテスト スイートをローカルで実行します。そのためには、プロジェクトに必要な唯一のランタイムである **[Node.js][nodejs]** が必要です。現在の **LTS** リリースをインストールしてください。

どのプラットフォームでも、公式インストーラーを使うのが最も簡単です。

1. Windows Terminal、macOS のターミナル、または普段使っているターミナルを開きます。
2. 次のコマンドを実行し、インストール済みの Node.js バージョンを確認します。

    ```shell
    node --version
    npm --version
    ```

3. どちらのコマンドも成功し、Node.js がプロジェクトの README と `package.json` に記載された要件を満たしている場合は、次のセクションに進みます。

> [!TIP]
> Node.js がインストールされていない場合や、更新が必要な場合にのみ、以降の手順を実行してください。

4. [Node.js のダウンロード ページ][node-download]を開きます。
5. 使用中のオペレーティング システム向けの **LTS** ビルドをダウンロードします。
6. インストーラーを実行し、既定の設定を使用します。Windows では **Add to PATH** を選択したままにします。
7. インストール後、新しいターミナルを開きます。
8. 新しいターミナルで次を実行し、インストールを確認します。

    ```bash
    node --version
    npm --version
    ```

9. インストールした Node.js と npm のバージョンが表示されます。

> [!IMPORTANT]
> Copilot app の agent からも `node --version` と `npm --version` を実行できる必要があります。普段使っているターミナルで成功しても、app から使えるとは限りません。Node.js のインストール後や PATH を変更した後は、app を完全に終了して再起動してください。セッション内でコマンドが失敗する場合は、続行する前に[セッション環境のトラブルシューティング][session-environment]を確認します。

> [!IMPORTANT]
> 各 worktree には、プロジェクトの依存関係と E2E チェック用の Playwright Chromium も必要です。worktree を準備する際は Tailspin Toys リポジトリの README に従い、インストールの要求は内容を確認してから承認してください。

## ラボ リポジトリをセットアップする

Tailspin Toys プロジェクトの自分用コピーを使います。[テンプレート リポジトリ][template-repository]から作成してください。新しいリポジトリにはラボに必要なファイルがすべて含まれており、app のインストール時に接続します。

1. 新しいブラウザー ウィンドウで、このラボの GitHub リポジトリ `https://github.com/github-samples/tailspin-toys` を開きます。
2. ラボ用リポジトリ ページで **Use this template** ボタンを選択し、**Create a new repository** を選んで自分用のコピーを作成します。

    ![ドロップダウンで Create a new repository が選択された Use this template ボタン](../../../_images/app-0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの案内に従います。それ以外の場合は、GitHub Copilot を利用できる Organization に新しいリポジトリを作成します。

    ![github-samples/tailspin-toys がテンプレートに設定され、リポジトリ名が入力された Create a new repository フォーム](../../../_images/app-0-create-repository.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) をメモします。ラボで後ほど使用します。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub issue のバックログが自動的に作成されます。ワークショップを通じてこの issue を使うため、自分で登録する必要はありません。

新しいワークショップ テンプレートのコピーを使ってください。リポジトリの instruction、アプリケーション コード、テスト、quality-checks skill、既存の canvas 拡張機能が含まれています。ワークショップでは skill をカスタマイズし、QA agent を作成します。古いコピーを使用する場合は、必要なファイルが含まれていることを進行役に確認してください。

## まとめと次のステップ

準備ができました。このレッスンでは、次のことを行いました。

- プロジェクトをコンピューター上でビルドおよびテストできるように Node.js をインストールしました。
- テンプレートから Tailspin Toys リポジトリの自分用コピーを作成しました。

次は [GitHub Copilot app をインストールし][next-lesson]、作成したリポジトリを接続して workspace を確認します。

## リソース

- [Node.js をダウンロード][node-download]
- [テンプレートからリポジトリを作成する][template-repository]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#issue-からセッションを開始する
