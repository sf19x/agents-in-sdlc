---
title: "レッスン 8 - 機能 PR の作成とマージ"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能、指示、スキルの更新、品質保証 (QA) プロファイル、テストは同じブランチに保存されています。まとめてレビューしてプルリクエストを開きます。星評価のプルリクエスト (PR) は手動でマージしましたが、今度は **Agent Merge** にプロセスを管理させます。

> [!NOTE]
> 通常は機能、指示、スキル、QA エージェントを複数の PR に分けます。このワークショップでは作業を簡略化するため、フィルターと品質に関する一連の作業を1つの会話とブランチで進め、この PR にまとめました。

このレッスンでは、次のことを行います。

- Agent Merge と、マージライフサイクルを自動化する方法を確認します。
- 機能全体の変更と検証の根拠を調べます。
- フィルター機能の PR を作成します。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能の作業では Copilot を使って計画、実装、検証を行いました。Tailspin Toys では開発者がマージを許可する権限を保ったまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** はプルリクエストを取り込むために必要な残りの作業を自動化します。有効にすると、Copilot は CI チェックの失敗、レビューコメントへの対応、必要なリベースなど、PR を妨げている問題を解決し、リポジトリが許可している場合は GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge に任せる前に、作業をレビューして準備ができていることを確認します。

## Agent Merge で PR を管理する

すべてのコードを作成したので、まとめてレビューし、PR を作成して、残りを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで PR を作成します。
5. `/pr agentmerge` で Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを確認します。すべて成功するとマージされます。
7. Copilot から「PR #14 was squash-merged successfully.」などのメッセージが届いたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。ブロックされた場合は報告された理由を確認し、リポジトリで許可されている場合はレビュー済みのマージを手動で完了します。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- Agent Merge と、マージライフサイクルを自動化する方法を学びました。
- 機能全体の変更と検証の根拠を確認しました。
- フィルター機能の PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は[コンテキスト、モデル、共有、任意のクラウドへの委任に使える Copilot CLI のスラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
