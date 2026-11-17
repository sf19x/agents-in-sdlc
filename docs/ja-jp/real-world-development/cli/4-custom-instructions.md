---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加してフィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときはコンテキストが重要です。特定の方法でタスクを進める必要がある場合、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]には、求めるコードだけでなく、コードの構成方法も記述できます。フィルター機能を構築したので、Copilot が使った指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパス別の指示ファイルがエージェントに適用される方法を確認します。
- コーディング標準を確実に守るため、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発のガイドラインと要件があります。

- コメントにはコードの言い換えではなく、意図や自明でない判断を記述します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TypeScript ドキュメントコメント (TSDoc) または JavaScript ドキュメントコメント (JSDoc) を付けます。該当する場合は注入可能な `db` 引数も記載します。
- 再利用可能な Astro コンポーネントには `Props` の契約を記述し、関連コードが変わったらコメントも更新します。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらの慣例に沿ってタスクを実行できるようにします。

## 指示ファイル

カスタム指示では、コーディングスタイルや要件などのコンテキストを Copilot に渡し、より適切な提案やコード生成を促します。リポジトリ全体、または特定のファイル種別に指示を設定できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送られる単一の指示ファイルです。多くのリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、言語、ファイル種別、タスクごとのガイドラインを記載します。

> [!NOTE]
> 指示形式とサポート状況は環境によって異なります。形式に依存する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトに含まれる指示を確認し、変更前の状態を把握します。

1. Codespace に戻ります。
2. Codespaces のエディターで `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。この指示はリポジトリ内の Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開き、Astro、Drizzle データ層、テストなどの指示を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` が指示の適用対象を決める glob であることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存プロジェクトファイルへのリンクを確認します。大きな指示を再利用可能なファイルに分け、例を Copilot に示せます。

## チームのガイダンスに合わせて指示を更新する

既存の指示に、生成される TypeScript に TSDoc コメントを付けるルールを追加します。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクション (35行目付近) を探します。
2. 最後の箇条書きに次の項目を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。フィルター機能のコンテキストを保ったまま、新しい指示を読み込ませます。

1. フィルターの会話が開いている場合は `/exit` で終了します。会話は自動的に保存されます。
2. リポジトリのルートで `game-filters-cli` ブランチから会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合はフィルターの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新後のガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- リポジトリの指示とパス別の指示ファイルがエージェントに適用される方法を確認しました。
- コーディング標準を確実に守るため、指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
