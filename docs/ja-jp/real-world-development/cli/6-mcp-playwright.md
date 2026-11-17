---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーからフィルター機能を調べます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスを使ったり、Copilot に追加の自動化機能を提供したりする必要があります。Model Context Protocol (MCP) サーバーを使うと、Copilot CLI に組み込まれていないツールやサービスも利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を理解します。
- 未設定の場合は Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルター機能を確認します。

## シナリオ

単体テストやエンドツーエンドテストは重要ですが、UI の更新を検証するには実際に操作する必要があります。利用者と同じようにサイトを操作できるよう Copilot に許可すると、変更が期待どおりに動くか確認できます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部ツールやサービスと通信するための仕組みです。MCP を使うと、エージェントは最新の情報にアクセスし、開発者に代わって操作できます。

ツールやリソースには、AI エージェントと外部サービスを仲介する MCP サーバー経由でアクセスします。各 MCP サーバーは、エージェントが利用できるツールとリソースのセットを提供します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理するアプリケーション プログラミング インターフェイス (API) にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright によるブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub はエコシステム内の検索や貢献を促進する [MCP registry][mcp-registry] をホストしています。

> [!CAUTION]
> MCP サーバーは他の依存関係と同様に扱ってください。利用前にソースコードと公開元を確認し、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot が利用者のようにサイトを操作できるよう追加します。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに設定されている場合は、このセットアップを飛ばして次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認して <kbd>Enter</kbd>、次に <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) で新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を確認するよう Copilot に依頼する

これまでは機能が動作することを手動で確認しました。今度は Copilot で利用可能な Playwright MCP サーバーを使います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常、Copilot は MCP サーバーを自動的に選択するため、明示的に指定する必要はありません。使うツールを把握している場合は、指定すると結果の一貫性を高め、トークンを節約できます。

2. Copilot がブラウザーで機能を確認する手順を示す様子を見ます。
3. レポートを読み、すべて期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright でサイトを操作してサーバーを停止し、結果を報告します。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を学びました。
- 未設定の場合は Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を確認しました。

次は[QA カスタムエージェントを作成し][next-lesson]、スキルとブラウザーツールを専門的な役割にまとめます。

## リソース

- [MCP とは何か、なぜ話題なのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
