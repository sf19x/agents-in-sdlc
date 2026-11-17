---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用可能なカスタマイズ、関連リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys で一連のワークフローを進め、GitHub Copilot CLI を使いました。具体的には、次のことを行いました。

- Codespace を準備して Copilot CLI をインストールし、プロジェクトを確認して既存のフィルター Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター Issue から始め、Plan モードで進め方を定め、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルターを確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザー検証の根拠を評価しました。
- フィルター機能全体をレビューし、フィルター PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、クラウドへの任意の委任に使うスラッシュコマンドを確認しました。

## リリースした内容

ワークショップでは、更新済みの `main` からそれぞれ独自のブランチを作り、2つの PR を作成しました。

1. **星評価:** 既存の `starRating` と未評価状態をゲームカードに表示します。
2. **フィルターと品質ワークフロー:** フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` の報告形式を変更し、QA プロファイルと関連テストを追加します。

フィルターの計画から PR 作成までは同じ会話とブランチを使いました。ワークショップを簡潔にするため、作業を1つの PR にまとめています。

## 異なる検証方法

自動テスト、自分でのブラウザー確認、MCP を通じた Copilot のブラウザー確認を行いました。`quality-checks` スキルは単体テスト、lint、型チェックを実行して新しい形式で報告しました。QA は要件とテストカバレッジのレビューにその結果を加えました。

追加したテストは実際の不足を補うものである必要があります。追加テストが不要という QA の結果も適切な場合があります。マージを許可する前にコードと根拠を確認し、変更後は関係する検証を再実行します。

## ベストプラクティス

Copilot に与えるコンテキストとツールが作業内容を左右します。このワークショップでは指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらを会話間で再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し作業を説明し、カスタムエージェントは専門的な役割を定め、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。構築前の検討には **Plan**、範囲の限られた変更で人が関与するには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業には高性能なモデルを選びます。

基盤と同じくらいコンテキストも重要です。何を、なぜ、どのように作るかを明確に伝えると、出力が大きく変わります。

## さらに確認する

ワークフローの主要な部分を終えました。Copilot CLI の機能として、次のコマンドも確認できます。

- `/review` はコードレビューエージェントに変更を分析させます。
- `/rubber-duck` は問題を説明し、別の視点を得るのに使います。
- `/fleet` は独立したサブタスクを並行して調整します。
- `/worktree` は別のタスクを独立した worktree で扱います。
- `/delegate` はタスクを Copilot cloud agent に送ります。

## 次のステップ

どのツールも、使い続けることが上達につながります。本番コード、趣味のコード、作ろうと思いながら手を付けていない小さなアプリなどで活用してください。チームと学びを共有し、ドキュメントも確認しましょう。

ターミナルから Tailspin Toys を拡張するには、任意の[Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境も確認するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を参照してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
