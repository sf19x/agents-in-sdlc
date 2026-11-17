---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前の演習: カスタム指示][previous-lesson] |
|:--|

多くの組織では、未解決の問題、更新が必要なレガシーコード、バックログに残った機能要望など、技術的負債に取り組む時間が不足しています。GitHub Copilot cloud agent はコード更新や機能追加などを自律的に実行します。作業が終わると、人がレビューできるドラフトのプルリクエスト (PR) を作成します。時間のかかる作業を任せることで、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを行います。

- コード生成環境をカスタマイズします。
- 操作を安全に行う方法を確認します。
- 範囲を明確にした Issue の重要性を学びます。
- Issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には対処したい技術的負債があります。アプリケーションのエクスポート関数にはドキュメントがありません。まず、すべての関数に TypeScript ドキュメントコメント (TSDoc) を追加します。

また、デザインチームはゲームの発見性を高めたいと考えています。各ゲームの詳細ページに同じカテゴリーの関連ゲームを表示し、UX の受け入れテストに使える状態にします。これは作業を妨げているものの、他の Issue より優先度は低いタスクです。

こうしたタスクは後回しになりがちです。Copilot cloud agent に割り当てれば、非同期で処理されている間に別の作業を進め、後から結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人の開発者と同様にバックグラウンドで作業できます。たとえば [GitHub Issue を Copilot に割り当て][assign-issue]ると、Copilot はドラフト PR を作成し、環境を準備してタスクを始めます。作業中または完了後にセッションを確認でき、解決案をレビューする準備ができると PR で通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot を使っても、考える作業を省略できるわけではありません。単純に見える操作にも複雑さがあります。[cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階的に進め、学び、試し、必要に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習ではカスタム指示とその効果を確認しました。cloud agent に作業を割り当てる前に、リポジトリに用意された次のファイルを読み取り専用で確認します。

- `.github/copilot-instructions.md`: **Code standards** を確認し、TypeScript の慣例と TSDoc コメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` frontmatter が `**/*.test.ts` に指示を適用することを確認します。

次のセクションで `Code lacks documentation` Issue を割り当てた後、PR に TSDoc コメント、TypeScript の慣例、ファイルヘッダーが反映されているか確認します。

## Copilot cloud agent の開発環境を設定する

Copilot はソフトウェアエンジニアのように作業するため、コード生成環境とセットアップスクリプトが必要です。cloud agent は GitHub Actions の環境を使います。`.github/workflows/copilot-setup-steps.yml` に設定した[セットアップワークフロー][setup-workflow]で、作業前に開発ツールや依存関係を準備できます。この演習では Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、単一 Astro アプリ用にマイグレーションとシードを行った SQLite データベースを利用できるよう、あらかじめ設定されています。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *各手順*に必要な最小限の権限を設定します。Copilot の操作には専用トークンが付与されます。
    permissions:
      # 依存関係のインストールなどでセットアップ中にリポジトリをクローンする場合は `contents: read` 権限が必要です。クローンしない場合は手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使えるよう、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

このファイルは通常の GitHub Actions ワークフローと似ていますが、重要な点があります。

- `copilot-setup-steps` という1つのジョブが、Copilot が PR 作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーがあるため、Copilot の実行を待たずにリポジトリの **Actions** タブからワークフローを手動テストできます。

## ドキュメントを追加する

ドキュメントは重要ですが、古いままになったり、存在しなかったりするプロジェクトもあります。保守や新しい開発者の参加を難しくする技術的負債です。Copilot にドキュメント作成を依頼すると、バックグラウンドで作業し、後の演習で結果をレビューできます。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot への割り当て詳細](../../../_images/ex4-assign-copilot-details.png)
10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` のようなタイトルの新しいプルリクエスト (PR) を開きます。一覧にない場合は少し待ってからブラウザーを更新します。
12. 数分後、Copilot が TODO リストを作成したことを確認します。

> [!NOTE]
> TODO リストが PR に表示されるまで数分かかることがあります。Copilot は先ほどのワークフローを実行して環境を作り、プロジェクトを分析して、問題への最適な対応方法を検討しています。

13. リストと実行予定のタスクを確認します。
14. PR のタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッション画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された状態が表示されない場合は、ブラウザーを更新してください。

16. ライブセッションをスクロールし、Copilot がコードと状態を調べ、計画を検討し、コードを作成する様子を確認します。

この作業には数分かかる場合があります。cloud agent の利点は非同期でタスクを進められることです。別の Issue も割り当てて、アプリの機能開発に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

cloud agent は複数の作業を分けて進められます。デザインチーム向けの関連ゲーム機能を割り当て、その間に別の作業を進めます。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue ダイアログを開きます。
4. 空のテンプレートを使うため、**Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

Issue の最初のコメントに 👀 が表示されたら、Copilot が作業を開始しています。

![Copilot が Issue に対応していることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼を処理しています。ソフトウェアエンジニアと同じように cloud agent は自律的に作業するため、常時監視する必要はありません。完了後に結果をレビューします。次はカスタムエージェントを作成して使います。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使うと、Issue を割り当てて非同期に処理できます。技術的負債の解消、新機能の作成、フレームワーク間のコード移行などを依頼できます。この演習では、次のことを確認しました。

- コード生成環境のカスタマイズ
- 操作を安全に行う方法
- 範囲を明確にした Issue の重要性
- Issue を Copilot に割り当てる方法

cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントを作成して使います。[cloud agent は MCP サーバー][cloud-agent-mcp]とカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次の演習: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
