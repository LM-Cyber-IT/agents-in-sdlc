---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加のリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連の開発作業を進めました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、登録済みのフィルター Issue を見つけた。
- 星評価を追加し、転送ブラウザーで結果をレビューして、最初のプルリクエスト (PR) を手動でマージした。
- フィルターの Issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューした。
- カスタム命令でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして単体テスト、lint、型チェックを実行した。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認した。
- 品質保証 (QA) 用のカスタム エージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの証拠を評価した。
- フィルターの変更全体をレビューし、Agent Merge によるフィルターの PR のマージを許可した。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュ コマンドを確認した。

## リリースしたもの

このワークショップでは、それぞれ更新した `main` から作成したブランチで、2 つの PR を作成します。

1. **星評価**: 既存の `starRating` と、評価がない場合の明示的な表示をゲーム カードに追加する。
2. **フィルターと品質ワークフロー**: フィルター機能を実装し、命令を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して関連テストを追加する。

フィルターの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡略化するため、作業を 1 つの PR にまとめています。

## 検証方法の違い

自動テスト、自分で行うブラウザー確認、MCP を使った Copilot のブラウザー確認という複数の方法でコードを検証しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、指定した形式で結果を報告します。QA は、要件とテスト範囲のレビューにそれらの結果をまとめてから PR に進みます。

テストは実際の不足を補うために追加します。QA の実行で追加のテストが不要と判断されても、正しい場合があります。マージを許可する前に、コードと検証の根拠を確認し、変更後は影響を受ける検証をやり直してください。

## ベスト プラクティス

Copilot に提供するコンテキストとツールは、作業結果に影響します。このワークショップでは、命令を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームのニーズに応じて調整してください。命令は規約を設定し、スキルは繰り返し行う作業を記述し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。実装前に方針を検討するには **Plan**、範囲を絞った変更でやり取りを続けるには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同様に、コンテキストも重要です。何を、なぜ、どのように作るのかを明確に説明すると、結果が大きく変わります。

## さらに確認する

基本的なワークフローを終えました。Copilot CLI には、ほかにも次のような機能があります。

- `/review` でコードレビュー エージェントに変更の分析を依頼する。
- `/rubber-duck` で問題を相談し、別の視点を得る。
- `/fleet` で独立したサブタスクを並列に実行する。
- `/worktree` で別のタスクを分離する。
- `/delegate` で Copilot cloud agent にタスクを送る。

## 次のステップ

ツールを上達させるには、実際に使い続けるのが最善です。運用コード、個人のコード、作りたいと思いながら手を付けていなかった小さなアプリなどで活用してください。チームと知見を共有し、互いに学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
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
