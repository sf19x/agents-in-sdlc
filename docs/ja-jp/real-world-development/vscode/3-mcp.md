---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前の演習: エージェントモード][previous-lesson] · [次の演習: カスタムエージェント →][next-lesson]

エージェントモードでフィルター機能を構築しました。プルリクエストを開く前に、ブラウザーで動作を確認します。**Model Context Protocol (MCP) に対応した Playwright サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストし、その後ブランチを公開してプルリクエスト (PR) を作成します。

この演習では次のことを行います。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- ブラウザーでフィルター機能を手動テストするよう Copilot に依頼します。
- ブランチを公開してフィルター作業の PR を開きます。

## Model Context Protocol (MCP) とは

エージェントモードはエディターの外部にアクセスできると、さらに便利になります。Model Context Protocol (MCP) は Copilot が外部ツールやサービスと通信するための標準です。

![エージェントモードがコンテキスト、大規模言語モデル (LLM)、MCP サーバーや VS Code 拡張機能のツールと連携する仕組みを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) によって、AI エージェントは外部ツールやサービスとリアルタイムに通信できます。リソースを使って最新情報にアクセスしたり、ツールを使って開発者に代わって操作したりできます。

MCP サーバーは AI エージェントと外部サービスの間を仲介します。既存 API や npm パッケージなどのローカルツールへの通信を管理し、それぞれ異なるツールとリソースを提供します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリ、Issue、プルリクエストを管理する API にアクセスします。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright でブラウザーを自動操作し、ページ移動、フォーム入力、ボタン選択などを行います。

ほかにもさまざまなツールを使う MCP サーバーがあります。GitHub は検索やエコシステムへの貢献を促進する [MCP registry](https://github.com/mcp) をホストしています。

> [!CAUTION]
> MCP サーバーは他の依存関係と同様に扱ってください。利用前にソースコードと公開元を確認し、セキュリティへの影響を検討します。信頼できるサーバーだけを使い、機密リソースや操作へのアクセス許可に注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` は VS Code ワークスペースで使える MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` サーバーが設定されていることを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` エントリはプロジェクトテンプレートに含まれているため、前の演習で Copilot が Issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルへのインストールは不要です。VS Code は HTTP で接続し、初回利用時に GitHub 認証を求めます。

## Playwright MCP サーバーを追加する

[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できるブラウザーを提供します。

1. `.vscode/mcp.json` で `github` と並べて `playwright` エントリを追加し、次のようにします。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` サーバーと異なり、`playwright` はローカルサーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` は画面を表示せずにブラウザーを動かす設定で、デスクトップ環境のない Codespace で必要です。

> [!NOTE]
> Tailspin Toys はエンドツーエンドテストに Playwright を使うため、必要なブラウザーはすでにインストールされている場合が多くあります。ブラウザーがないと Copilot が報告した場合は、`npx playwright install chromium` を実行させて再試行します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを必要に応じて起動します。初めてツールを使うときにサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に表示される **Start** を選択し、サーバーを起動して設定を確認します。
2. 信頼の確認が求められたら、構成を確認してサーバーを信頼するよう選択します。

手動での起動は任意です。省略した場合、次の手順でツールが必要になったときに VS Code が起動し、信頼を確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]が接続され、アプリが実行中であることを確認してから、Copilot に次を依頼します。エージェントはページを開き、移動してフィルターを適用し、結果を読み取って報告できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を利用します。エージェントはボタン、リンク、リスト項目など、ラベル付きの構造化要素を調べます。機能確認がアクセシビリティの簡易確認にもなります。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバー経由でブラウザーを起動し、各手順を実行して結果を報告します。Issue の受け入れ条件に照らして報告を確認し、問題があれば PR を開く前に追加質問をしたり、コードの修正を依頼したりします。

> [!NOTE]
> テストではアプリを `http://localhost:4321` で起動しておく必要があります。開発サーバーを止めた場合は、プロンプトを送る前に再起動します。Playwright MCP の初回利用時にはブラウザーのダウンロードが必要な場合があります。見つからない場合は `npx playwright install chromium` を実行させて再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能の動作を確認したので、チームのレビュー用にプルリクエスト (PR) を開きます。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot の変更をレビューします。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンでコミットメッセージを生成します。

   ![Copilot の変更を表示する Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub CLI から PR を作成する方法もありますが、GitHub ツールが接続された Copilot に依頼します。

1. Copilot Chat パネルに移動し、**New Chat** で新しいセッションを開始します。
2. GitHub ツールを使えるよう、エージェント一覧で **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報の取得や操作を許可します。GitHub ツールを初めて使うときは認証を求められる場合があります。
5. Copilot が Issue を探し、適切なものに関連付けて PR を作成したことを確認します。
6. Copilot が作成したリンクを選び、PR をレビューします。**まだマージしないでください**。

## まとめと次のステップ

この演習では次のことを行いました。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーでフィルター機能を手動テストしました。
- ブランチを公開してフィルター作業の PR を開きました。

MCP は Copilot を拡張する方法の1つです。次は[カスタムエージェントを作成し][next-lesson]、アクセシビリティレビューなどの専門タスクを効率化します。

## リソース

- [MCP とは何か、なぜ話題なのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
