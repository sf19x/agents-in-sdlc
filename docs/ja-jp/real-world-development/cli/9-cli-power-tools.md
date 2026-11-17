---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキスト管理、モデル選択、セッション共有、クラウドへの任意の委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様、GitHub Copilot CLI には対話に使うスラッシュコマンドが多数あります。高度な機能、内部情報、追加設定を利用できます。これまで `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` を使いました。ここではほかの便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で Copilot の会話コンテキスト管理を確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## シナリオ

CLI の主要なワークフローを終えたので、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent] への任意の委任などを確認します。

## Copilot CLI のコンテキストを調べる

大規模または複雑なタスクでは、モデルのコンテキストウィンドウが上限に達する場合があります。Copilot CLI は必要に応じて会話を自動的に圧縮します。スラッシュコマンドを使って状態を確認したり、自分で圧縮したりできます。

1. Codespace に戻り、Copilot CLI が起動していない場合はリポジトリのルートから起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き容量の内訳を確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキストウィンドウがいっぱいになると、Copilot CLI は自動的に圧縮します。タイミングを選びたい場合は `/compact` を使います。関係のないタスクに移るときに新しい会話を始めるには、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも違います。Copilot CLI では利用できるモデルを表示して選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 使用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを維持するか、別のモデルを選ぶか、<kbd>Esc</kbd> で一覧を閉じます。

## セッションを共有する

チームで学びを共有すると、AI ツールの活用方法を互いに高められます。`/share` はセッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みバージョンの `/share` オプションを確認します。
2. 共有する場合は、ローカルの Markdown に出力する `/share file` など、目的に合った出力先を選びます。
3. エクスポート内容を送信または公開する前に確認します。プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有するつもりのないリポジトリや会話の内容を公開しないでください。

## 任意: 作業の拡張や委任

ワークショップの主要部分は完了しました。大きなタスクでは、次の Copilot CLI コマンドも利用できます。

- `/fleet` は独立したサブタスクをサブエージェントに割り当て、並列実行できます。
- `/worktree` は別のタスク用に独立した Git worktree を作成できます。
- `/delegate` はタスクを Copilot cloud agent に送り、非同期で処理してプルリクエストを作成できます。

これらのコマンドは追加の worktree やリモート作業を作る場合があるため、任意です。試す場合は新しく範囲を明確にしたタスクから始め、通常の手順で結果をレビューします。非同期エージェントの作業をさらに確認するには、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- `/context` と `/compact` で Copilot の会話コンテキスト管理を確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

Copilot CLI にはほかにも多くのコマンドがあります。次は[学んだ内容と次の手順を振り返り][next-lesson]、ワークショップを締めくくります。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
