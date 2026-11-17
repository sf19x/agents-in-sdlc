---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前の演習: 前提条件][previous-lesson] · [次の演習: Copilot cloud agent →][next-lesson]

生成 AI を使うときはコンテキストが重要です。タスクの進め方や Copilot が知るべき背景情報を、Copilot が参照できるようにします。[指示ファイル][instruction-files]を使うと、何を実装するかだけでなく、どのように実装するかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス別の指示ファイルが、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスと異なり、ここでは変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期で動作するため、リアルタイムで並べて効果を示すのが難しいためです。このハーネスの後半で、指示ファイルがプルリクエストに与えた影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には開発ガイドラインがあります。

- データ層には常に単体テストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- コードには TypeScript ドキュメントコメント (TSDoc) を追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントを追加します。

指示ファイルを使い、Copilot がこの方針に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示はコーディングスタイルや要件などのコンテキストを Copilot に渡し、提案やコード生成を導きます。リポジトリ全体、または特定のファイル種別に指示を設定できます。

- `.github/copilot-instructions.md` は、リポジトリに関する**すべての**リクエストに送信する指示ファイルです。技術スタック、プロジェクト概要、ベストプラクティスなど、広く関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に対するガイドラインを記載します。TypeScript や Astro、UI コンポーネント、単体テストなどに使えます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成に使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルを使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルも[サポートしています][custom-instructions-support]。

### 指示ファイルを管理するためのベストプラクティス

スタータープロジェクトの例を確認し、指示を整理する方法を学びます。

- `copilot-instructions.md` には、プロジェクトの説明、構成、全体的なコーディング標準などを記述します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データ層などのファイル種別やタスク固有の指示を記載します。
- 自然な言葉で、明確なガイダンスとコードの良い例・悪い例を示します。

指示ファイルの作成方法は1つではありません。プロジェクトや環境に合う方法を試して見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、指示ファイルを整備します。このプロジェクトには [UI 更新][ui-instructions]や [Astro][astro-instructions] などの指示があります。
>
> Copilot に指示ファイルを作成させることもできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、操作は環境によって異なります。
>
> テンプレートや出発点を探すには、指示ファイルやカスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

リポジトリに含まれる `copilot-instructions.md` と `*.instructions.md` をエディターまたは GitHub の Web UI で確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などの項目を確認します。**Code standards** の **GitHub Actions Workflows** の指示も、Copilot とのやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ層、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` がリポジトリルートからの glob を定義していることを確認します。たとえば `**/*.test.ts` に一致する TypeScript テストに適用されます。
5. このプロジェクトの単体テスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾を開き、他の指示ファイルやプロジェクト内の例へのリンクを確認します。パスはリポジトリルートではなく指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** にはコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次に TSDoc コメントとファイルヘッダーの規則を追加します。

## リポジトリ標準を追加する

コードに TSDoc コメントとファイルヘッダーを付けるプロジェクト全体のルールを追加します。cloud agent は後の演習でこの指示を使います。

1. Codespace のターミナルで作業ブランチを作成します。cloud agent は Issue の対象ブランチから指示を読みますが、ここでは編集を個別にコミットします。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. `.github/copilot-instructions.md` を開きます。
3. 27行目付近の **Code formatting requirements** を探し、現在はコード内のドキュメント規則がないことを確認します。
4. 既存の標準の直下に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. `copilot-instructions.md` を保存します。

> [!TIP]
> リポジトリ全体の指示は `.github/copilot-instructions.md`、言語やタスク固有の指示は `*.instructions.md` に記載できます。今回追加したドキュメントコメント規則はプロジェクト全体に関わるため、リポジトリレベルのファイルが適しています。

## 指示の更新をコミット、プッシュ、マージする

次の演習で Issue を cloud agent に割り当てると、Copilot は `main` からブランチを作成します。変更した指示を使わせるには、`main` にマージする必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開いてマージします。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチの作成を省略し、`main` に直接コミットできます。ここでは実際のプロジェクトでの手順に合わせています。

## まとめと次のステップ

この演習では次のことを行いました。

- リポジトリのカスタム指示とパス別の指示ファイルが Copilot に適用される方法を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。

後の演習で、cloud agent が作成する PR にその標準が反映されることを確認します。

次は[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
