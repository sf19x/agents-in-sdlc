---
title: "演習 5: GitHub Copilot の作業を反復する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前の演習: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このハンズオンでは、ユーザー体験や機能を改善する複数のタスクを GitHub Copilot に依頼しました。コードへのドキュメント追加、デザインチーム向けの関連ゲーム機能、ハイコントラストとライトモードを含むアクセシビリティ機能です。変更を確認し、必要ならフィードバックを送って改善します。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成コードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全員にレビューを依頼する前に、ドキュメント、関連ゲーム、アクセシビリティ機能に関する Copilot の提案を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期かつ自律的に作業するため、安全性を保つ制約があります。

- Copilot はリポジトリへの読み取りアクセスと、作業用ブランチへの書き込みアクセスだけを持ちます。
- GitHub Actions 内に一時的な専用環境を作成して実行します。
- GitHub Actions ワークフローの実行には、人による承認が必要です。
- [外部リソースへのアクセスは既定で制限されます](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーへのアクセスも制限されます。

## 生成されたドキュメントをレビューする

最初に、コードへドキュメントを追加したプルリクエスト (PR) を GitHub.com の標準的な PR 画面で確認します。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は MCP サーバーなどの外部リソースに既定では制限付きでアクセスするため、これは想定された動作です。必要であれば、[cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルの PR を開きます。

> [!NOTE]
> Copilot が作業中の場合、PR のタイトルに **[WIP]** が付きます。完了するまで待ちます。数分かかる場合があります。

4. 準備ができたら **Files changed** タブを選び、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 追加された TSDoc コメントなどのドキュメントを確認します。変更内容は状況によって異なります。演習 2 で確認した指示ファイルによる TSDoc、TypeScript の慣例、ファイルヘッダーに注目します。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローがキューに追加されたことを確認します。単一 Astro アプリのプロジェクトチェックが成功するまで待ちます。数分かかる場合があります。

## GitHub Copilot に変更を依頼する

PR のコメントやコードへのインラインコメントで **@copilot** をメンションし、追加の変更を依頼できます。結果は確定的ではないため、必ず同じ依頼文を使う必要はありません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、内容を説明するコメントを追加します。
- TypeScript と Astro ファイルに TSDoc コメントを追加します。
- Astro アプリの構成を説明する README を作成します。

1. これらの例などを使って、ドキュメントへの変更を依頼するコメントを追加し、通常の利用者と同様に **@copilot** をメンションします。
2. **View Session** を選択して Copilot の作業を確認します。Copilot は更新用の新しいセッションを開始します。
3. **Back to pull request** を選択して PR に戻ります。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了したら、PR に新しいコミットがあることを確認します。
5. **Files changed** タブで変更をレビューします。

納得できるまで反復できます。準備ができたら、ドラフト PR をレビュー可能な状態にして `main` ブランチへマージします。

![PR をレビュー可能な状態にする操作](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue 用に Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を開きます。
4. **Files changed** タブで生成コードをレビューします。
5. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ち表示を確認します。
7. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. チェック欄にワークフローがキューに追加され、単一 Astro アプリのプロジェクトチェックが成功するまで待ちます。
9. **任意:** Codespace でこのブランチをチェックアウトし、関連ゲーム機能を手動テストできます。Codespace のターミナルを開き、`<branch-name>` を Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

必要なら PR の **Conversation** タブにコメントし、関連ゲームの表示件数やファイルヘッダー、TSDoc コメントなどの変更を依頼できます。カスタム指示を更新する前に割り当てた Issue であることを考慮し、Copilot に追加セッションを開始させます。

## アクセシビリティ機能をレビューする

最後に、カスタムアクセシビリティエージェントが実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 で追加依頼したライトモードが含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を開きます。

> [!NOTE]
> Copilot が作業中の場合は PR に **[WIP]** が表示されます。作業が完了するまで待ちます。

4. **Files changed** タブを選択して、生成コードをレビューします。
5. 実装を確認し、次の点に注意します。
   - モード切り替えの UI コンポーネント
   - 利用者の設定を保持する local storage
   - ハイコントラストとライトモードの CSS またはスタイル
   - ARIA ラベル、キーボード操作、その他のアクセシビリティ属性
   - モード切り替えを管理する JavaScript/TypeScript
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ち表示を確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. チェック欄にワークフローがキューに追加され、単一 Astro アプリのプロジェクトチェックが成功するまで待ちます。
10. **任意:** Codespace で `<branch-name>` を Copilot が作成したブランチ名に置き換えて、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   アプリを起動し、ブラウザーでハイコントラストとライトモードを試し、ページを再読み込みしても設定が保持されることを確認します。

カスタムアクセシビリティエージェントは、アクセシビリティのベストプラクティスに沿って機能を実装するよう Copilot を導きました。懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼します。

## 任意の演習 — 委任を続ける

cloud agent には実際のバックログを割り当て、別の作業に取り組む方法が適しています。リポジトリに Issue を追加し、Copilot に割り当てる練習をします。例:

- ゲーム詳細ページに支援者向けの関心フォームを作成します。
- ゲーム一覧にページネーションを実装します。
- データアクセスヘルパーに入力検証とエラー処理を追加します。

## まとめ

Cloud agent ハーネスを完了しました。カスタム指示を確認して効果を確かめ、Issue を cloud agent に割り当てて非同期の環境準備、計画、実行を確認しました。アクセシビリティ用のカスタムエージェントを作成して使い、Copilot agents ページでセッションを監視・案内し、PR をレビューして **@copilot** に変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。さらに確認する場合は、次のハーネスも活用してください。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat のエージェントモードと MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan、Autopilot、スキル、QA エージェント、Playwright MCP、Agent Merge、任意の `/delegate` を使い、ターミナルから2つの変更をリリースします。

自分のリポジトリでも、次のことを試してください。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てます。
- 別の分野向けのカスタムエージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前の演習: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
