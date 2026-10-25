---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を体験します。変更の計画、コードの作成、動作のテスト、プルリクエストのレビュー、エージェントを使った作業管理を段階的に進めます。

## シナリオ

架空の会社 Tailspin Toys に新しく加わった開発者として、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームに取り組みます。チームのバックログは GitHub Issue として登録済みです。フィルターやページネーションなどの機能開発に加え、アクセシビリティやコーディング規約などの品質向上も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、開発者が作業する環境で利用できます。自分に合った環境を選んでください。各ワークショップは個別のセットアップから始まり、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

GitHub Copilot CLI（コマンドライン インターフェイス）をターミナルで使います。Plan モードと Autopilot モード、命令、スキル、カスタム エージェント、Playwright の Model Context Protocol (MCP)、Agent Merge、実用的なスラッシュ コマンドを扱います。

### [GitHub Copilot app][app]

デスクトップ アプリでエージェント セッションを並行して実行し、セッション モードを切り替え、Canvas で共同作業し、GitHub Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業を案内し、進行状況を監視して、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使用します。エディターを離れずに Copilot Chat のエージェント モード、MCP サーバー、カスタム エージェントを扱います。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
