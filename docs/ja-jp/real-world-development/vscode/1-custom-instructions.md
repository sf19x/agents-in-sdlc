---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前の演習: 前提条件][previous-lesson] · [次の演習: エージェントモード →][next-lesson]

生成 AI を使うときはコンテキストが重要です。特定の方法でタスクを進める場合や、Copilot に背景情報を伝える場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]は、コード生成のガイダンスを伝える方法の1つです。

この演習では次のことを行います。

- リポジトリのカスタム指示とパス別指示で、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 現在の指示でパブリッシャーヘルパーを生成します。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加します。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映されることを確認します。
- 指示の更新とフィルター機能の基盤を `main` にコミットし、後の演習で新しい指示を使えるようにします。

> [!CAUTION]
> 生成コードは指定した標準から外れる場合があります。Copilot の結果は確定的ではありません。この演習では、指示の更新後に傾向が変わることを確認します。文字単位で同じ出力を期待するものではありません。

## 指示ファイル

### シナリオ

Tailspin Toys には開発ガイドラインがあります。データ層には単体テストを用意し、ユーザーインターフェイス (UI) は現代的なダークモードにし、コードには TypeScript ドキュメントコメント (TSDoc)、各ファイルの先頭にはファイルの役割を説明するコメントを追加します。指示ファイルを使い、Copilot がこの方針に沿って作業できるようにします。

### カスタム指示

カスタム指示はコーディングスタイルや要件などのコンテキストを Copilot に渡し、提案やコード生成を導きます。リポジトリ全体、または特定のファイル種別に指示を設定できます。

- `.github/copilot-instructions.md` は、リポジトリに関する**すべての**リクエストに送信する指示ファイルです。技術スタック、プロジェクトの概要、ベストプラクティスなど、広く関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、TypeScript や Astro などの言語、UI コンポーネント作成、単体テストなどのタスクやファイル種別に使います。

> [!NOTE]
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat のコード生成に使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルを使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルも[サポートしています][custom-instructions-support]。

### 指示ファイルを管理するためのベストプラクティス

スタータープロジェクトの例を参考に、指示を整理します。

- `copilot-instructions.md` はプロジェクトの概要、構成、全体的なコーディング標準などに絞ります。
- `*.instructions.md` は、単体テスト、Astro コンポーネント、データ層などのファイル種別やタスク固有の指示に使います。
- 明確な自然言語で指示し、コードの良い例・悪い例を示します。

指示ファイルの作成方法は1つではありません。プロジェクトで試し、適した方法を見つけます。

> [!TIP]
> このプロジェクトには [UI 更新][ui-instructions]や [Astro][astro-instructions] などの指示があります。Copilot に指示ファイルを作成させることもできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、操作は環境によって異なります。テンプレートには [awesome-copilot][awesome-copilot] を参照してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリにある指示ファイルをエディターまたは GitHub の Web UI で確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などを確認します。**Code standards** 内の **GitHub Actions Workflows** の指示も Copilot とのやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ層、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` がリポジトリルートを基準とする glob であることを確認します。たとえば `**/*.test.ts` に一致する TypeScript テストに適用されます。
5. 単体テスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾を開き、`unit-tests.instructions.md` などの指示や既存ファイルへのリンクを確認します。これにより大きな指示を再利用可能なファイルに分け、Copilot に例を示せます。パスは指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** にはコーディング標準が記載されていますが、コード内のドキュメント規則はありません。次に TSDoc コメントとファイルヘッダーを追加します。

## 変更用ブランチを作成する

前の演習の Codespace に戻り、変更用ブランチを作成します。

1. Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) でターミナルを開きます。
3. ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選び、新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

指示の効果を比べるため、まず現在の指示のままプロンプトを送信し、指示を更新した後に同じプロンプトを再送します。

1. 前の演習から開いているエディタータブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を示します。
3. Chat ビューのエージェント一覧で **Agent** を選び、ファイル変更を許可します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを確認し、ヘルパーファイルやテストなどを更新する様子を確認します。
6. 提案されたヘルパーが、`db` クライアントを最初の引数に取り、型付きパブリッシャー配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約によるものです。
7. 提案されたコードに TSDoc コメントとファイルヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確定的ではないため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。指示を更新した後に一貫性が高まることがこの演習の目的です。

## リポジトリ標準を追加する

`.github/copilot-instructions.md` にプロジェクト全体のコーディング標準を追加します。

1. `.github/copilot-instructions.md` を開きます。
2. 27行目付近の **Code formatting requirements** を探します。生成されたヘルパーにドキュメントコメントがない理由を確認します。
3. 既存の標準の直下に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. ファイルを保存します。

> [!TIP]
> リポジトリ全体の標準は `.github/copilot-instructions.md`、言語やタスク固有の指示は `*.instructions.md` に記載できます。今回のドキュメントコメント規則はプロジェクト全体に関わるため、リポジトリレベルのファイルが適しています。

## 同じプロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択してコンテキストをクリアします。
2. `src/lib/publishers.ts` を開き、Copilot が適切なファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディングプラットフォームのパブリッシャーデータアクセスヘルパー。
    * データベースからパブリッシャー情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * すべてのパブリッシャーの ID と名前の一覧を返します。
    *
    * @param db - Drizzle データベースクライアント。
    * @returns パブリッシャーオブジェクトの配列に解決する Promise。
    */
   ```

新しい標準に従うよう Copilot を導き、後の演習で使う実際のコードに適用しました。

## 指示をコミットしてブランチをプッシュする

指示ファイルもほかのファイルと同様にソース管理します。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) でターミナルを開きます。
2. 次のコマンドで指示とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 指示の更新、ヘルパーの基盤、Copilot が変更した関連ファイル (テストなど) をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準 Issue に関連付けた PR を作成します。GitHub ツールが接続されているので、Copilot に Issue を探して PR を作成し、マージを依頼できます。

1. Codespace の Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) で Copilot Chat ビューを開き、エージェント一覧で **Agent** が選択されていることを確認します。
3. 指示ファイルの更新とパブリッシャーヘルパーの基盤の両方を説明し、現在のブランチから PR を作成するよう依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成する様子を確認します。
5. GitHub 操作の **Allow** を求められた場合は、コマンドを確認して適切であれば許可します。
6. PR が作成されたら、次のプロンプトでマージしてローカルブランチを `main` に戻すよう依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub 操作とシェルコマンドの **Allow** を求められたら、操作を確認して適切であれば許可します。

GitHub Copilot の支援で PR を作成してマージしました。

## まとめと次のステップ

この演習では次のことを行いました。

- プロジェクト固有のコンテキスト、ガイドライン、ドキュメント標準が指示ファイルを通じて Copilot に伝わる方法を確認しました。
- 現在の指示でコード生成プロンプトを送り、パブリッシャーヘルパーを作成しました。
- `.github/copilot-instructions.md` に標準を追加しました。
- 同じプロンプトを再実行し、新しい標準がコードに反映されることを確認しました。
- 指示の更新とヘルパーの基盤を `main` にコミットし、次の演習で使えるようにしました。

次は[エージェントモード][next-lesson]で指示を活用し、コードベース全体に機能を追加します。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
