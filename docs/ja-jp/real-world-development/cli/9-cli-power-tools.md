---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、cloud agent への任意の委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には操作に使うスラッシュ コマンドが数多く用意されています。高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次の作業を行います。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認する。
- `/model` を使って、利用可能なモデルを調べる。
- `/share` でセッションをエクスポートまたは共有する方法を確認する。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認する。

## シナリオ

CLI を使った基本的なワークフローが完了しました。ここでは、コンテキストの管理、モデルの切り替え、セッションの共有、および [Copilot cloud agent][about-cloud-agent] への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルのコンテキスト ウィンドウの上限に達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュ コマンドで状態を調べたり、手動で圧縮したりできます。

1. Codespace に戻り、Copilot CLI が開いていない場合は、リポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム命令、ツール、メッセージ、空き領域へのコンテキストの割り当てを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. 再度 `/context` を入力し、結果を比較します。会話がすでに短い場合、大きな変化がないこともあります。

> [!NOTE]
> コンテキスト ウィンドウがいっぱいになると、Copilot CLI は自動的にコンテキストを圧縮します。タイミングを指定したい場合は `/compact` を使います。関係のない別のタスクに切り替えて新しい会話を始める場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルにはそれぞれ異なる強みがあり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルを一覧表示して選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを維持するか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力して知見を共有すると、人工知能 (AI) ツールの活用方法を改善できます。`/share` コマンドを使うと、セッションを Markdown ファイルまたはハイパーテキスト マークアップ言語 (HTML) ファイルにエクスポートしたり、共有可能なリンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みバージョンで利用できる `/share` オプションを確認します。
2. セッションを共有する場合は、ローカルの Markdown ファイルにエクスポートする `/share file` など、目的に合った方法を選びます。
3. エクスポートした内容を他の人に送信または公開する前に確認します。エクスポートにはプロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有する予定のないリポジトリや会話の内容を公開しないでください。

## 任意: 作業を拡張または委任する

ワークショップの基本部分は完了です。Copilot CLI には、大きなタスク向けのコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに割り当て、並列実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、非同期で作業し、プルリクエストを作成する場合がある Copilot cloud agent にタスクを送信できます。

これらのコマンドは追加の worktree やリモート作業を作成する可能性があるため、任意です。試す前に、範囲を明確にした新しいタスクを始め、通常のワークフローで結果をレビューしてください。非同期エージェントによる作業をさらに確認したい場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

スラッシュ コマンドを使って、Copilot CLI を構成し、セッションを共有し、内部の動作を確認しました。このレッスンでは、次の作業を行いました。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認した。
- `/model` を使って、利用可能なモデルを調べた。
- `/share` でセッションをエクスポートまたは共有する方法を確認した。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認した。

Copilot CLI にはほかにも多くのスラッシュ コマンドがあります。次は、ここまでに学んだ内容を[振り返り、今後の手順を確認し][next-lesson]ましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
