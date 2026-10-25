---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウドで非同期に作業できます。GitHub 上で作業を割り当てると、cloud agent がバックグラウンドでリポジトリを調べて変更を加え、プルリクエストを作成します。その間、開発者は別の作業を続けられます。

各演習では、まず cloud agent が従うカスタム命令を追加し、次に GitHub Issue を割り当てて実装を任せます。カスタム エージェントを確認して利用し、エージェント ダッシュボードからセッションを監視して指示を加え、最後に作成されたプルリクエストをレビューして成果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. カスタム命令][ex1] | コンテキスト | cloud agent が従うカスタム命令を追加する |
| [2. Cloud agent][ex2] | 非同期エージェント | GitHub Issue を Copilot cloud agent に割り当てる |
| [3. カスタム エージェント][ex3] | 専門エージェント | カスタム エージェントを確認して使用する |
| [4. エージェントの管理][ex4] | 監視 | エージェント セッションを監視し、指示を加える |
| [5. 作業の反復][ex5] | レビュー | プルリクエストをレビューし、Copilot の作業を改善して次の手順を選ぶ |

## 前提条件

このワークショップを始める前に、次の準備ができていることを確認してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] アカウントまたは Organization で GitHub Copilot cloud agent が有効になっていること

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、Model Context Protocol (MCP)、コードレビュー、コマンドライン インターフェイス (CLI) の機能が含まれているため、どの harness でも完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> cloud agent は Copilot Free を除く **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Business または Enterprise の場合、管理者が Organization で有効にする必要があります。

> [!NOTE]
> この harness では MCP を扱いません。Copilot で MCP サーバーを使う方法は、[CLI harness](../cli/) または [Visual Studio Code (VS Code) harness](../vscode/) を参照してください。

## はじめる

まず、リポジトリと Codespace を準備します。

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
