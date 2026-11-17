---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、アプリケーション、共通のバックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更の計画、コードの作成、動作のテスト、プルリクエストのレビュー、エージェントを使った作業管理を繰り返し実践します。

## シナリオ

ゲーム開発をテーマにしたクラウドファンディングを提供する架空の会社、Tailspin Toys の新しい開発者として作業します。チームのバックログは GitHub Issue として登録済みで、すぐに取り組めます。フィルタリングやページネーションなどの機能開発に加え、アクセシビリティやコーディング標準の改善も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、開発者が選んだ環境で利用できます。自分の作業方法に合った環境を選んでください。どのワークショップも独自のセットアップから始まり、共通の Tailspin Toys シナリオを使います。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan と Autopilot の各モード、指示、スキル、カスタムエージェント、Playwright MCP、Agent Merge、実用的なスラッシュコマンドを扱います。

### [GitHub Copilot app][app]

デスクトップアプリでエージェントセッションを並行して実行し、セッションモードを切り替え、キャンバスで共同作業を行い、GitHub Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業を案内して進捗を確認し、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに Copilot Chat のエージェントモード、MCP サーバー、カスタムエージェントを扱います。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
