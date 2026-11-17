---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

小さな更新でも、複数ファイルの変更やテスト実行が必要になることがあります。必要なファイルを探し、変更し、テストし、問題をデバッグして、漏れた変更を探す作業が続きます。

Copilot Agent Mode を使うと、IDE 内でより自律的に作業できます。既存プロジェクトを調べて複数のファイルを更新し、テストなどのタスクを実行し、見つかった問題を修正します。Agent Mode でサイトに機能を追加します。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途向けです。エージェントモードは IDE 内で動作し、すばやいフィードバックと対話を可能にします。cloud agent は Issue とプルリクエストを通じて非同期に作業します。

この演習では次のことを学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数ファイルを変更する方法。
- ユーザーインターフェイス (UI) とデータ層にまたがる機能の実装方法。
- マージ前に生成された変更とテストをレビューする方法。

## シナリオ

ゲームが増えてきたため、カテゴリーとパブリッシャーで絞り込めるようにします。前の演習で追加したパブリッシャーヘルパーを使い、Copilot Agent Mode でデータ層、UI、テストの残りを完成させます。

## Tailspin Toys の Web サイトを実行する

変更前にサイトを確認して、現在の機能を把握します。

Tailspin Toys は開発者をテーマにしたボードゲームのクラウドファンディングサイトです。ゲーム一覧と詳細を表示します。サイトはビルド時にハイパーテキストマークアップ言語 (HTML) の静的ページを生成する単一の Astro アプリです。ページは frontmatter 内で Drizzle のオブジェクト関係マッピング (ORM) を通じてローカル SQLite データベースを直接参照します。独立したバックエンドのアプリケーション プログラミング インターフェイス (API) やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、対話機能は標準ドキュメントオブジェクトモデル (DOM) API を使う小さな Astro `<script>` で実装します。

### Web サイトを起動する

`npm run dev` タスクで Astro 開発サーバーを起動します。

1. Codespace に戻ります。前の演習の後、`main` にいることを確認します。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押してターミナルを開きます。
3. フィルター作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro サーバーの準備ができると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くダイアログが表示された場合は **x** を選択して閉じます。

5. ターミナルの Astro サーバーアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択してサイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動転送されます。これは Codespace のサーバーに接続するプライベートトンネルです。

### サイトを確認する

ゲーム一覧にはゲームのタイトル、画像、説明があります。ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリーなどを表示する詳細ページが開きます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選び、新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

テンプレートからプロジェクトを作成すると、Issue のバックログも自動的に用意されています。Copilot に内容を表示させます。

1. Chat ビューのエージェント一覧で **Agent** を選択します。Agent は複数ファイルを調べて変更し、ターミナルコマンドを実行し、ツールを使います。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このモデルを特定のモデルより推奨する意図はありません。ワークショップ作成時に Claude Sonnet 4.5 を使用し、結果の一貫性を高めるために記載しています。大規模言語モデル (LLM) の出力は確定的ではないため、例と異なる提案が出る場合があります。これは想定された動作です。

> [!NOTE]
> Copilot は異なる MCP コマンドを使う場合がありますが、タスクを完了できるはずです。

3. 次のプロンプトを Copilot に送信して、バックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Issue 一覧を表示するコマンドを実行するには **Continue** を選択します。
5. Copilot が生成した Issue 一覧を確認します。

## 指示ファイルを確認する

コード生成を依頼する前に、Copilot にコンテキストを与える `.github/instructions/ui.instructions.md` を確認します。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関するガイダンスを確認します。アーキテクチャ、コンポーネント設計、テスト容易性、アクセシビリティの原則、Astro・Tailwind CSS・Drizzle の指示へのリンクが含まれます。

> [!TIP]
> 指示ファイルは他の指示やプロジェクトファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、大きな内容を小さなファイルに分け、例やテンプレートを示せます。

## フィルター機能を実装する

フィルター機能には、データ層 (`src/lib/`) のロジック、テスト、ゲーム一覧ページの UI が必要です。マージ前にテストを実行し、合格を確認します。

1. 現在の Copilot 会話を続けるか、**New Chat** で新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択します。

   ![UI 指示ファイルを選択する画面](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント一覧で **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログの Issue を基に機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、データ層、UI、テストの関連ファイルを探して変更し、テストを実行する様子を確認します。

   ![Copilot がプロジェクトファイルを調べる画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> ファイルの調査、変更、テスト実行には数分かかることがあります。作業中、エディターに **Keep** と **Undo** が表示されても、処理中に選択する必要はありません。完了後に変更ごとの選択肢が表示されます。

7. Copilot から求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求める画面](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したりテストが失敗したりしても問題ありません。Copilot はエラーを検出しなくなるまでコード生成とテストを繰り返します。

   ![Copilot Agent Mode の完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. 実行中の Web サイトに戻り、新機能を試します。
11. 動作とコードを確認したら、Copilot Chat で **Keep** を選択します。

## まとめと次のステップ

この演習では次のことを行いました。

- Copilot Agent Mode が UI とデータ層にまたがる機能を実装する方法を確認しました。
- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定して変更を調整する様子を確認しました。
- マージ前に生成された変更とテストをレビューしました。

次は[Playwright MCP サーバーで機能をテストし][next-lesson]、PR を開きます。

### 追加演習 — ページネーションを実装する

ゲーム一覧が増えると、ページネーションが必要になります。この演習のスキルを使って、Copilot にページネーションを依頼します。既存の指示に従うこと、ページサイズを利用者が選ぶか固定するか決めること、必要なガイダンスをプロンプトで示すこと、対話を続けて調整することを検討します。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
