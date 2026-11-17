---
title: "演習 5 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前の演習: カスタムエージェント][previous-lesson] |
|:--|

エージェントモードの Copilot は、コードベースを調べ、変更を提案し、ファイルを編集してコマンドを実行します。VS Code の Copilot Chat では、ツール呼び出しやファイル編集をリアルタイムで確認できます。差分を確認して個別の変更を承認または破棄し、エディターを離れずに追加プロンプトで作業を調整できます。

この演習では、アクセシビリティエージェントの作業をリアルタイムで監視し、ファイル変更をレビューして承認し、同じセッションにライトモードを追加するよう依頼します。

## シナリオ

[演習 4][previous-lesson]で使ったアクセシビリティカスタムエージェントは、ハイコントラストモードを実装しています。作業中、デザインチームから既定のテーマが暗すぎる利用者向けにライトモードも必要だと要望がありました。新しい会話を始めず、実行中のセッションに追加作業を依頼します。

## Copilot の進捗を監視する

Copilot Chat にはエージェントが調べたファイル、実行したコマンド、提案した編集が表示されます。編集は差分と **Keep** / **Undo** の操作とともにチャットに表示されます。

1. 演習 4 の Copilot Chat ビューに戻ります。
2. エージェントの作業中にチャットをスクロールし、ツール呼び出し、ファイルの読み書き、実行を求めるターミナルコマンドを確認します。
3. ファイル編集が提案されたら、差分を確認します。
   - 変更を採用するには **Keep** を選択します。
   - 変更を戻して再試行させるには **Undo** を選択します。
4. チャット上部の **Files changed** を開き、セッション中にエージェントが変更したファイルを確認します。

> [!NOTE]
> ターミナルコマンドの実行などに許可が必要な場合、エージェントは **Allow** を表示して一時停止します。内容を確認し、許可してもよいコマンドだけ承認してください。

5. エージェントがハイコントラストモードを実装し、変更をまとめるまで待ちます。

## ライトモードを追加するようエージェントに依頼する

ハイコントラストモードを同じ会話で拡張します。これまでの作業、変更したコンポーネント、スタイル、local storage のパターンをエージェントが保持しています。

1. Copilot Chat に次のフォローアッププロンプトを送信します。

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. エージェントが既存の変更を確認し、ライトモードの追加を提案する様子を確認します。
3. 各差分を確認し、**Keep** または **Undo** を使います。
4. エージェントが意図しない方向に進んだ場合は、チャット入力欄横の **Stop** で停止し、明確なフォローアップを送信します。

> [!TIP]
> 実行中のセッションを案内すると、最初からやり直すより短時間で調整できます。たとえば CSS の簡素化、既存設定パネルへの統合、両方の切り替え用キーボードショートカットの追加などを依頼します。

## 作業内容をレビューする

コミットする前に、エージェントが変更したファイルを確認します。

1. VS Code で **Source Control** ビューを開きます。
2. 変更されたファイルの一覧を確認します。Astro コンポーネント、スタイル、関連テストなどが含まれる場合があります。
3. 変更ファイルをいくつか開き、差分を確認します。カスタムエージェントのアクセシビリティ要件、Accessible Rich Internet Applications (ARIA) 属性、キーボード操作、セマンティック HTML、local storage による設定保持が反映されていることを確認します。
4. 問題がなければ **Source Control** パネルから変更をステージしてコミットします。次の演習で公開します。

## まとめと次のステップ

このレッスンでは次のことを行いました。

- エージェントの作業をリアルタイムで監視しました。
- 提案されたファイル編集をレビューして承認しました。
- 実行中のセッションに追加指示を送り、ライトモードを追加しました。

次はアクセシビリティの作業をプルリクエストとして公開し、このハンズオンでカスタムエージェントが生成した変更をレビューします。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [エージェントモードを使う][agent-mode]

---

| [← 前の演習: カスタムエージェント][previous-lesson] | [次の演習: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
