---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習で、Copilot cloud agent にユーザー体験の改善や機能追加に関する3つのタスクを依頼しました。非同期かつ自律的に動作する cloud agent でも、作業を監視することは重要です。

GitHub.com の[エージェントページ][agents-page]など、タスクを管理する方法があります。ここでは、開いているプルリクエスト (PR) があるすべてのエージェントタスクを確認し、実行中のセッションを調べて、作業の進め方を案内できます。

このレッスンでは、次のことを行います。

- エージェントページで cloud agent のタスクを監視します。
- 実行中のセッションに追加機能を依頼します。

## シナリオ

ハイコントラストモードの作業中、チームはライトモードも必要だと気付きました。サイトのスタイルと切り替え機能を更新しているため、同じエージェントにライトモードも実装するよう依頼します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン領域と左側の一覧で、割り当てたタスクを確認します。コードドキュメントの追加、ゲームを変更する API の生成、サイトのハイコントラストモードなどが表示されます。
3. 実行中のタスクを選択し、Copilot の作業を確認します。リポジトリのチェックアウト、環境の作成、MCP サーバーの設定、タスクを完了する操作などが含まれる場合があります。

> [!NOTE]
> 表示される手順は Copilot の状態や作業方法によって異なります。

4. 右側の PR ペインに表示されるプルリクエストと変更ファイルも確認します。

## cloud agent の作業を方向付ける

実行中のタスクを確認したので、ハイコントラストモードの作業にライトモードの切り替えも追加するよう依頼します。

1. ハイコントラストモードのセッションを選択します。タイトルは Copilot が付けた名前や作業の状態によって異なります。

   ![ミッションコントロールのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが終わり作業が始まるまで数分待ちます。作業開始後はメッセージが表示されます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![エージェントページで作業中の cloud agent に指示するダイアログ](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に加えたことを確認します。

## Copilot の作業を待つ

Copilot は現在の手順を終えた後、更新されたタスクにライトモードを追加します。非同期の処理には数分かかる場合があります。その間に、これまでに学んだ内容を振り返ります。

## まとめと次のステップ

このレッスンでは次のことを行いました。

- Copilot agents ページを使って cloud agent のタスクを監視しました。
- 実行中のセッションに追加機能を依頼しました。

Copilot agents ページは cloud agent のタスクを監視・案内する中心的な場所です。進行中と完了済みのタスク、実施された作業を確認し、実行中のタスクに追加の指示を送れます。

Copilot がアクセシビリティ機能を実装している間に、次は[作成されたプルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前の演習: カスタムエージェント][previous-lesson] | [次の演習: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
