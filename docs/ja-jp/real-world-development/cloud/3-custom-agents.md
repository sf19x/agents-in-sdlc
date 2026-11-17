---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた専門 AI アシスタントを作成できます。リポジトリの `.github/agents` にある Markdown ファイルでエージェントを定義し、指示、ベストプラクティス、コーディングパターン、専門知識を与えます。チームの知識を再利用可能なエージェントにできます。たとえば [Web Content Accessibility Guidelines (WCAG)][wcag] に沿うアクセシビリティエージェント、安全なコーディングを支援するセキュリティエージェント、一貫したテストを保つテストエージェントです。

エージェントはプロジェクト内の `.github/agents`、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]は、どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。簡潔に言うと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、スキルや他のエージェントを調整して作業できます。このハンズオンでは、アクセシビリティエージェントがサイトをガイドラインに沿ってレビュー・更新します。その作業の一部として、プルリクエストワークフローやテスト実行などのスキルを呼び出す場合があります。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では次のことを確認します。

- カスタムエージェントを定義する方法。
- タスクをカスタムエージェントに割り当てる方法。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず誰もが使えるクラウドファンディングプラットフォームを目指しています。利用者から、現在のダークテーマは文字と背景のコントラストが不十分で読みにくいという意見が寄せられました。デザインチームは、利用者が切り替えられるハイコントラストモードを求めています。アクセシビリティは重要なため、カスタムエージェントを使って機能をすばやく作成します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに用意されています。Codespace に戻り、次の手順で内容を確認して Copilot への指示を把握します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには `name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、アクセシブルなサイトのコード生成に関する責任、ベストプラクティス、ハイパーテキストマークアップ言語 (HTML)、カスケーディングスタイルシート (CSS)、JavaScript の例、よくある問題を確認します。

## Issue を作成して割り当てる

Mission control は環境内のエージェントを操作する中心的な場所です。Copilot cloud agent にタスクを割り当て、作業を監視し、追加の指示を送れます。ここではハイコントラストモードの Issue を Copilot に割り当てます。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。
9. カスタムエージェント一覧から **Accessibility agent** を選択します。

   ![カスタムエージェントとして Accessibility agent を選んだ割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を始めることを確認します。

## まとめと次のステップ

この演習では次のことを行いました。

- カスタムエージェントを定義する方法を確認しました。
- ハイコントラストモードの Issue を作成し、Accessibility agent に割り当てました。

カスタムエージェントは特定のタスクや分野に特化したアシスタントです。次は[Mission control からエージェントセッションを監視して方向付け][next-lesson]します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを利用する準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを利用する準備][enterprise-custom-agents]

---

| [← 前の演習: GitHub Copilot cloud agent][previous-lesson] | [次の演習: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
