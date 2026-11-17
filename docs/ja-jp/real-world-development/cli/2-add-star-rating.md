---
title: "レッスン 2 - 星評価の追加: 小さな改善"
description: "Copilot CLI でゲームカードを変更し、転送されたブラウザーで確認して、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したので、次はプロジェクトに初めて変更を加えます。変更は小さくします。ゲームデータには星評価がありますが、ホームページのゲームカードには表示されていません。エージェントに既存の評価を表示させ、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで Copilot との会話を始めます。
- エージェントに小さな変更を依頼します。
- `/diff` で変更をレビューします。
- アプリを実行し、転送されたブラウザーで確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。しかし、ホームページのゲームカードにはタイトル、カテゴリー、パブリッシャー、説明しか表示されません。準備として、各カードに既存の評価を表示するようエージェントに依頼します。最初のセッションに適した、小規模で独立した変更です。

## 会話の構成

**会話**では、Copilot CLI とタスクに取り組みます。Copilot app と異なり、通常の CLI 会話では専用の worktree を作らず、ターミナルで現在チェックアウトしているリポジトリと Git ブランチを使います。保存した会話は後で再開できますが、ファイルとブランチは通常の Git 状態としてディスク上に残ります。

会話では、プロンプトとエージェントの応答、ファイルを調査・編集するエージェントのツール操作、`/diff` で確認できる変更の3つを確認できます。

## 会話を始めて変更を依頼する

機能の実装に向けて、新しい会話を開始します。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. Copilot CLI が起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. `/new` を入力して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲーム詳細の表示に使うファイルを見つけて必要なコードを作成します。これで Copilot CLI を使った新機能の追加は完了です。

## 差分をレビューする

小さな変更でも、AI が生成した変更はマージする前にレビューします。Copilot CLI で変更を確認します。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. 評価がある場合は数値が、`starRating` が `null` の場合は `No rating yet` がゲームカードに表示されることを確認します。
3. テストが両方の状態を確認していることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> Copilot を含む生成 AI ツールは決定論的ではないため、コードは例と異なる場合があります。特定の実装を期待するのではなく、動作を確認してください。

## 変更を確認する

コードを読むだけで動作すると判断せず、Codespaces で転送されたブラウザーから更新後のユーザーインターフェイス (UI) を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. ゲームカードに5段階の評価が表示されることを確認します。
4. Copilot に起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを開いてマージする

機能を実装できたので、プロジェクトにコードをマージするプルリクエスト (PR) を作成します。

1. 既定のエージェントに変更をコミットするよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。PR の作成時に Copilot CLI が既存のコミットをプッシュし、PR の URL を表示します。
3. Copilot CLI に表示された URL を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して PR を開きます。
4. 変更されたファイルとチェックをレビューします。
5. 準備ができたら **Merge pull request** を選択し、マージを確認します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って、初めての変更をリリースしました。具体的には、次のことを行いました。

- 機能ブランチで Copilot との会話を始めました。
- ゲームカードへの小さな変更をエージェントに依頼しました。
- `/diff` で変更をレビューしました。
- アプリを実行し、転送されたブラウザーで星評価を確認しました。
- 最初のプルリクエストを開いてマージしました。

次は[フィルター機能の Issue から始め、Plan と Autopilot モードを使って][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
