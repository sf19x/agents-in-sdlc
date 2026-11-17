---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で進め方を合意し、Autopilot で Issue に基づくフィルタリングを構築し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初は小さな機能を追加しました。より大きな変更には、さらに堅実なプロセスが必要です。GitHub Copilot CLI は組織の既存の作業方法に合わせて動作できるため、適切な方法で必要な機能を構築できます。このレッスンは一連のレッスンの最初です。Issue を基に機能を作り、コードの妥当性と機能の動作を確認し、プロジェクトにマージする、一般的なエージェント主導の開発プロセスを進めます。

> [!NOTE]
> 機能開発を続ける間は同じ会話とブランチを使います。通常はファイルの種類ごとにブランチや PR を分けますが、ここでは主な概念に集中するため手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub Issue から Copilot の会話を始めます。
- Plan モードで要件を定義します。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

続く作業では、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、Model Context Protocol (MCP) による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増えているため、利用者はカテゴリーとパブリッシャーでゲームを絞り込む必要があります。バックログの Issue には機能が記されていますが、カテゴリーの組み合わせなど、コーディング前に合意する詳細があります。Plan モードで判断をまとめてから、範囲を定めた実装を Autopilot に許可します。

## 背景

AI コーディングエージェントを開発に取り入れても、基本は変わりません。多くの開発者は次のような流れで作業します。

1. 実装内容を記載した Issue から始めます。
2. 実装計画を作成します。
3. コードを構築してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 詳細はチームや組織によって異なりますが、多くは上記の流れを基本にしています。

この標準的な進め方に沿うことで、AI が生成したコードが要件を満たし、手作業のコードと同じレビューを受けるようにできます。

## 会話モード

**会話モード**はエージェントに許可する自律性を制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントが変更を提案し、次に進む前に開発者の入力を待ちます。
- **Plan**: エージェントが先に計画を作成し、プロジェクトファイルの編集は行いません。
- **Autopilot**: エージェントがコードを書き、テストを実行して、タスクが完了したと判断するまで自律的に反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

フィルター機能に着手する前に、Codespace とリポジトリ、ターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新してフィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub の Issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画を立てると、機能の実装やタスクの実行方法を Copilot に任せる前に、進め方を定められます。複雑な作業では、事前に計画することをお勧めします。Plan モードに切り替えて計画を依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替え、プロンプトの下に **Plan** と表示されていることを確認します。
2. 先ほど追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、適切な方法を検討して計画を作成します。

3. フィルターの動作について質問された場合は、希望に基づいて回答します。唯一の正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上下にスクロールして確認します。
6. 判断と合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューできたら、Autopilot で実装します。Autopilot は Copilot がタスクを完了したと判断するまで反復します。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みバージョンに表示される同様の項目を選択します。
2. プロンプトの下に **Autopilot** と表示されていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があります。先に計画を確認してください。依存関係の不足やポートの競合が報告された場合は、チェック完了とみなす前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同様にマージ前にレビューします。コードを確認し、サイトを実行します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに移り、モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルターの実装とテストを調べます。
3. 結果を Issue と計画中の判断に照らし合わせます。
4. レビュー後、<kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードを確認したら、Codespaces の転送ポートからサイトを開いて動作を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画で合意したカテゴリー、パブリッシャー、および組み合わせでフィルターを試します。
4. リセットと結果が空の場合の動作が、Issue と合意内容に沿っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を構築し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から Copilot の会話を始めました。
- Plan モードで要件を定義しました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで手動検証しました。

次は[カスタム指示を使い][next-lesson]、コードが文書化された慣例に従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
