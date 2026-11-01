---
title: "レッスン 0 - 前提条件"
description: "Tailspin Toys の自分用コピーを作成し、Copilot CLI ワークショップ用に GitHub Codespace を準備します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI のレッスンを始める前に、必要な環境を準備します。Tailspin Toys リポジトリのコピーと [Codespace][codespaces] を作成します。次のレッスンでは、Codespace の統合ターミナルを使って Copilot CLI をインストールし、実行します。

このレッスンでは、次のことを行います。

- Tailspin Toys プロジェクトの自分用コピーをテンプレートから作成します。
- Codespace を作成し、プロジェクトの準備ができていることを確認します。

## ラボ リポジトリをセットアップする

Tailspin Toys プロジェクトの自分用コピーを使います。[テンプレート リポジトリ][tailspin-template]から作成してください。新しいリポジトリには、ラボに必要なファイルがすべて含まれています。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. **Use this template** を選択し、続けて **Create a new repository** を選択して、自分用のコピーを作成します。
3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの案内に従います。それ以外の場合は、GitHub Copilot を利用できる組織にリポジトリを作成します。
4. 作成したリポジトリのパス (`organization-or-user-name/repository-name`) をメモします。ワークショップの後半で使用します。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub issue のバックログが自動的に作成されます。ワークショップ全体を通してこの issue を使うため、自分で issue を登録する必要はありません。

instruction、アプリケーション コード、テスト、`quality-checks` skill、ワークショップで使うバックログを含む、新しいワークショップ テンプレートのコピーを使用してください。古いコピーを使用する場合は、必要なファイルが含まれていることを進行役に確認してください。

## Codespace を作成する

次に、ワークショップの演習に使う Codespace を作成します。

[GitHub Codespaces][codespaces] は、ブラウザー内でコードの作成、実行、デバッグを行えるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートするエディターを利用できます。

1. 新しく作成したリポジトリを開きます。
2. **Code** を選択します。
3. **Codespaces** タブを選び、**Create codespace on main** を選択します。
4. Codespace のセットアップが完了するまで待ちます。テンプレートは、プロジェクトの依存関係、Playwright Chromium、ローカル データベースをインストールします。
5. **Do you trust the authors of the files in this folder?** と確認されたら、**Trust Folder & Continue** を選択します。
6. リポジトリ ルートでターミナルを開き、アプリケーションを起動します。

   ```bash
   npm run dev
   ```

7. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択し、Tailspin Toys のサイトが読み込まれることを確認します。
8. ターミナルに戻り、<kbd>Ctrl</kbd>+<kbd>C</kbd> で開発サーバーを停止します。

## まとめと次のステップ

準備ができました。このレッスンでは、次のことを行いました。

- Tailspin Toys プロジェクトの自分用コピーをテンプレートから作成しました。
- Codespace を作成し、プロジェクトの準備ができていることを確認しました。

次は Codespace に [GitHub Copilot CLI をインストール][next-lesson]し、GitHub アカウントで認証します。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces クイックスタート][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
