---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で実装方針を決め、Autopilot で Issue に基づくフィルター機能を構築し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に小さな機能を追加しましたが、大きな変更にはより確実なプロセスが必要です。GitHub Copilot CLI は、組織の既存の開発フローに沿って、適切なものを適切な方法で構築できるよう支援します。このレッスンから、Issue をもとに新機能を作り、コードの妥当性と機能の動作を確認し、プロジェクトにマージするまでのエージェント主導の開発プロセスを実践します。

> [!NOTE]
> 機能開発の一連の作業では、同じ会話とブランチを使います。通常はファイルの種類ごとにブランチやプルリクエスト (PR) を分けますが、ここでは主要な概念に集中できるよう簡略化しています。

このレッスンでは、次の作業を行います。

- GitHub Issue から Copilot との会話を開始する。
- Plan モードで要件を定義する。
- Autopilot モードで新機能を実装する。
- コードをレビューする。
- 転送ブラウザーで機能を手動検証する。

この機能の作業を続けながら、リポジトリの命令を更新し、既存の `quality-checks` スキルをカスタマイズし、Model Context Protocol (MCP) による検証を追加して、品質保証 (QA) エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、利用者がカテゴリや発行元でゲームを絞り込める必要があります。バックログの Issue には機能の概要がありますが、複数カテゴリの扱いなど、コーディング前に合意すべき詳細があります。Plan モードで決定事項を整理し、範囲を定めた実装を Autopilot に許可します。

## 背景

人工知能 (AI) コーディング エージェントを開発フローに取り入れても、基本的な手順は変わりません。多くの場合、開発者は次のように作業します。

1. 作業内容を記載した Issue から始める。
2. 実装計画を作成する。
3. コードを作成してレビューする。
4. テストを実行してコードを検証する。
5. 新機能を手動で検証する。
6. プルリクエスト (PR) を作成する。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージする。

> [!NOTE]
> 詳細はチームや組織によって異なりますが、多くの場合、上記の手順を基本としています。

標準的な手順に従うことで、AI が生成したコードが要件を満たし、手書きのコードと同じ確認プロセスを経るようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: 開発者とエージェントが共同で作業します。エージェントは変更を提案し、次に進む前に入力を待ちます。
- **Plan**: エージェントは最初に計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: エージェントはコードを書き、テストを実行し、タスクが完了するまで自律的に反復します。

最初に Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

フィルター機能の作業を始める前に、Codespace とターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が追加されます。`#` を使うと、GitHub 上の Issue やプルリクエストをコンテキストに追加できます。

## フィルター機能を計画する

計画を立てると、Copilot に実装を任せる前に機能の実装方法やタスクの進め方を決められます。複雑な作業では、時間をかけて計画を立てることをお勧めします。Plan モードに切り替えて、計画を依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替え、プロンプトの下にあるモード表示が **Plan** になっていることを確認します。
2. 前の手順で追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適な方法を検討して計画を作成します。

3. フィルターの動作について Copilot から質問されたら、希望に沿って回答します。唯一の正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を確認します。
6. 自分の決定と合わない部分があれば、修正を依頼します。

## Autopilot を承認する

計画を作成して確認したら、実装に進みます。Autopilot を使って Copilot に作業を任せます。Autopilot では、Copilot がタスクの完了を判断するまで問題に取り組み、反復します。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みバージョンで同様のラベルが付いた選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。先に計画を確認してください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェックを完了とみなす前にセットアップの問題を解決します。

## 実装をレビューして検証する

コードが生成されたら、ほかのコードと同様にマージ前のレビューが必要です。コードを確認し、サイトを実行して問題がないことを確かめます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** でないことを確認します。
2. `/diff` を入力し、フィルターの実装とテストを確認します。
3. 結果を Issue と計画時の決定事項に照らし合わせます。
4. コードを確認したら、<kbd>Esc</kbd> を押して差分ビューを閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を確認する

コードを確認したら、アプリを起動し、Codespaces の転送ポートからサイトを開いて動作を確かめます。

1. Copilot に次のプロンプトを送信してアプリを起動します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces にポート `4321` が使用可能と表示されたら、**Open in Browser** を選択します。
3. 計画で合意したカテゴリ フィルター、発行元フィルター、およびその組み合わせを試します。
4. リセットと結果がない場合の動作が、Issue と決定事項に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーの停止を依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を構築し、レビューしました。このレッスンでは、次の作業を行いました。

- GitHub Issue から Copilot との新しい会話を開始した。
- Plan モードで要件を定義した。
- Autopilot モードで新機能を実装した。
- コードをレビューした。
- 転送ブラウザーで機能を手動検証した。

次は[カスタム命令を使って][next-lesson]コード生成が文書化された規約に従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
