---
title: "レッスン 7 - QA エージェントの作成と使用"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザーからフィルター機能を確認しました。次は、明確な品質保証 (QA) プロセスを持つカスタムエージェントで機能をまとめます。

このレッスンでは、次のことを行います。

- カスタムエージェントが指示、スキル、MCP ツールを使う方法を確認します。
- 再利用可能な品質保証プロファイルを作成して調べます。
- QA エージェントを選択し、フィルター機能の Issue と照らして結果をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に要件、コード品質、自動チェック、テストカバレッジ、ブラウザーでの動作を一貫して確認したいと考えています。カスタムエージェントを使うと、QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の専門エージェントです。目的、指示、利用できるツールを記述します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチーム標準を説明し、`quality-checks` スキルは繰り返し実行できるチェックをまとめ、Playwright MCP はブラウザーツールを提供します。QA プロファイルは、それらを使って要件を評価し、結果を報告するよう Copilot に指示します。これらを置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、QA のチェック内容と制約を定義する再利用可能なプロファイルを Copilot に作成させます。

1. Codespace に戻り、フィルターの会話が Interactive モードであることを確認します。
2. 既定のエージェントにカスタムエージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、意図した QA の流れと制約が記述されていることを確認します。これにより、検証だけを求めたときに、範囲が広すぎるエージェントが機能を変更するのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は省略できますが、設定すると表示名が明確になります。
3. プロファイルの指示を読み、要件の確認、リポジトリの指示、`quality-checks` スキル、Playwright MCP が含まれていることを確認します。
4. QA が根拠を示し、実装コードの変更前に確認を求め、変更をコミットせず PR も開かないことを確認します。
5. 不足や制約の誤りがあれば、続行する前に既定のエージェントに修正を依頼します。

## Issue に対して QA を実行する

プロジェクトエージェントは会話開始時に読み込まれます。プロファイルを作成した後、同じフィルターの会話を再開して QA を選ぶと、Issue と計画の判断をそのまま利用できます。

1. 次のプロンプトを使ってエージェントを選択します。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成直後は使用可能なエージェント一覧に表示されない場合がありますが、上記のコマンドで有効にできます。

2. 次のプロンプトで機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業する様子を確認します。
4. 完了後、レポートを読みます。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- カスタムエージェントが指示、スキル、MCP ツールを使う方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して調べました。
- QA エージェントを選択し、フィルター機能の Issue と照らして結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートの準備ができました。次は[機能 PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
