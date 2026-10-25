---
title: "レッスン 8 - 機能のプルリクエスト (PR) を作成してマージする"
description: "フィルター、命令、スキル更新、品質保証 (QA) プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、命令の更新、スキルの更新、品質保証 (QA) プロファイル、テストはすべて 1 つのブランチに保存されています。まとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にマージのプロセスを任せます。

> [!NOTE]
> 通常は機能、命令の更新、スキルの更新、QA エージェントをいくつかの PR に分けます。このワークショップでは手順を簡略化するため、フィルター機能と品質確認の作業を 1 つの会話とブランチで進め、1 つの PR にまとめました。

このレッスンでは、次の作業を行います。

- Agent Merge と、マージ ライフサイクルを自動化する方法を確認する。
- 機能の変更全体と検証の根拠を確認する。
- フィルター機能の PR を作成する。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認する。

## シナリオ

フィルター機能の開発を通じて、Copilot で計画、実装、検証を行いました。Tailspin Toys では、マージの承認を開発者が管理しながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題に対応します。継続的インテグレーション (CI) チェックの失敗を修正し、レビュー コメントに対応し、必要に応じてリベースしたうえで、リポジトリで許可されている場合は GitHub の自動マージを有効にします。

これまでは、開発者自身が **Merge pull request** を選択していました。Agent Merge にこの作業を任せることもできますが、有効にする前に変更をレビューして、マージ可能な状態であることを確認してください。

## Agent Merge で PR を管理する

作成したコードをまとめてレビューし、PR を作成して、残りの手順を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべて成功すると、マージが実行されます。
7. Copilot が「PR #14 was squash-merged successfully.」などのメッセージを表示したことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリの設定、権限の不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されている場合は、レビュー済みのマージを手動で完了してください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの処理など、開発プロセスの一部を自動化しました。このレッスンでは、次の作業を行いました。

- Agent Merge と、マージ ライフサイクルを自動化する方法を確認した。
- 機能の変更全体と検証の根拠を確認した。
- フィルター機能の PR を作成した。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認した。

次は、コンテキスト、モデル、共有、cloud agent への任意の委任に便利な[Copilot CLI のスラッシュ コマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
