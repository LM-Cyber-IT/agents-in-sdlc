---
title: "レッスン 7 - 品質保証 (QA) エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた品質保証 (QA) カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上のフィルター機能を確認しました。次は、それらの機能を明確な QA プロセスを持つカスタム エージェントにまとめます。

このレッスンでは、次の作業を行います。

- カスタム エージェントが命令、スキル、Model Context Protocol (MCP) ツールと連携する方法を確認する。
- 再利用可能な品質保証プロファイルを作成して確認する。
- QA エージェントを選択し、フィルターの Issue に照らして結果をレビューする。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト範囲、ブラウザーでの動作を一貫してレビューしたいと考えています。カスタム エージェントを使うと、QA プロセスをまとめて実行し、再利用できるレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の専門的な人工知能 (AI) の役割です。プロファイルには、エージェントの目的、命令、利用可能なツールを記述します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使用したカスタマイズには、それぞれ異なる役割があります。リポジトリの命令はチームの規約を記述し、`quality-checks` スキルは繰り返し実行するチェックをまとめ、Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、要件の評価と結果報告のために、それらを Copilot がどう使うかを示します。各機能を置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、QA プロファイルを作成します。プロファイルには、QA で実行するチェックと守るべき制約を定義します。

1. Codespace に戻り、フィルターの会話が Interactive モードであることを確認します。
2. 既定のエージェントに次のプロンプトを送信して、新しいカスタム エージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

エージェントを使う前に、Copilot が意図した QA の手順と制約をプロファイルに反映したことを確認します。確認だけを依頼したのに、不完全または範囲の広すぎるエージェントが機能を変更してしまう事態を防ぎます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は省略できますが、設定すると分かりやすい表示名になります。
3. プロファイルの命令を読み、QA が要件から確認を始め、リポジトリの命令に従い、`quality-checks` スキルを実行し、Playwright MCP を使用することを確認します。
4. QA が根拠を示し、実装コードを変更する前に確認を求め、変更をコミットせず、PR も開かないことを確認します。
5. いずれかの役割や制約が不足している場合は、続行する前に既定のエージェントに修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成したら、フィルターの会話を再開して QA を選択します。これにより、Issue と計画時の決定事項を引き継げます。

1. 次のプロンプトを入力して QA エージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェントの一覧に表示されないことがあります。その場合も、上記のコマンドで有効にできます。

2. 次のプロンプトを送信し、機能をレビューするよう QA エージェントに依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントの作業を確認します。
4. 作業が終わったら、レポートを読みます。

## まとめと次のステップ

再利用できる専門的な役割をワークフローに追加し、その結果をレビューしました。このレッスンでは、次の作業を行いました。

- カスタム エージェントが命令、スキル、MCP ツールと連携する方法を確認した。
- 再利用可能な品質保証プロファイルを作成して確認した。
- QA エージェントを選択し、フィルターの Issue に照らして結果をレビューした。

実装、スキルの更新、QA プロファイル、テスト、検証レポートをレビューする準備が整いました。次は、これらを[機能の PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
