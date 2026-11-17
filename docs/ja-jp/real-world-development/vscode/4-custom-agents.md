---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]は、開発ワークフローの特定のタスクや分野に合わせた専門 AI アシスタントです。リポジトリの `.github/agents` にある Markdown ファイルでエージェントを定義し、指示、ベストプラクティス、コーディングパターン、専門知識を与えます。チームの知識を再利用可能なエージェントにできます。たとえば [Web Content Accessibility Guidelines (WCAG)][wcag] に沿うアクセシビリティエージェント、安全なコーディングを支援するセキュリティエージェント、一貫したテストを保つテストエージェントです。

エージェントはプロジェクト内の `.github/agents`、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]は、どちらも Markdown ファイルで定義し、AI に作業方法を伝えます。簡潔に言うと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、スキルや他のエージェントを調整します。この演習のアクセシビリティエージェントは、ガイドラインに沿ってサイトをレビュー・更新します。その作業の一部として、PR ワークフローやテスト実行などのスキルを呼び出す場合があります。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず誰もが使えるクラウドファンディングプラットフォームを目指しています。現在のダークテーマはコントラストが不十分で読みにくいという利用者の声があります。デザインチームは、利用者が切り替えられるハイコントラストモードを求めています。アクセシビリティは重要なため、カスタムエージェントで機能を実装します。

この演習では次のことを行います。

- 既存のアクセシビリティ用カスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用エージェントはすでに用意されています。内容を確認して Copilot への指示を把握します。

1. Codespace に戻り、ターミナルを開いて `main` からアクセシビリティ作業用のブランチを作成します。演習 3 のフィルター PR とは分けて作業します。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. `.github/agents/accessibility.md` を開きます。
3. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには `name` と `description` を含む frontmatter が必要です。

4. 続くセクションを読み、アクセシブルなサイトのコード生成に関する責任、ベストプラクティス、ハイパーテキストマークアップ言語 (HTML)、カスケーディングスタイルシート (CSS)、JavaScript の例、よくある問題を確認します。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat は `.github/agents` に定義したエージェントを、チャットビュー下部の一覧に表示します。エージェントを選ぶと、その指示とツールに沿った会話を始められます。

1. Codespace に戻り、Copilot Chat を開いて **New Chat** を選択します。
2. Chat ビューのエージェント一覧で **Agent** を選択します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

3. Chat ビュー下部のエージェント一覧を開きます。既定では **default** が選択されています。
4. 一覧から **Accessibility agent** を選択します。
5. 次のプロンプトを送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

6. Copilot Chat がコードベースを調べ、変更を提案して適用する様子を確認します。各編集はファイルパスと差分とともにチャットに表示されます。
7. 変更されたファイルに合わせて **Files changed** の表示が更新されることを確認します。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot は Astro コンポーネント、CSS、関連テストなどを実際に変更します。

次の演習で作業を確認し、方向付けします。

## まとめと次のステップ

この演習では次のことを行いました。

- 既存のアクセシビリティ用カスタムエージェントを確認しました。
- Copilot Chat のエージェントモードでアクセシビリティエージェントを使いました。

[GitHub Copilot のカスタムエージェント][custom-agents]を使うと、チームの知識や標準を再利用可能なエージェントにまとめられます。

次は[エージェントの作業を監視して方向付け][next-lesson]、変更を確認し、同じセッションにライトモードの切り替え機能を追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前の演習: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次の演習: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
