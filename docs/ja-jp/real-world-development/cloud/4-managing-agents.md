---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

直前の演習では、ユーザー エクスペリエンスの改善と機能追加に関する 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクの進行状況を監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。GitHub.com の[エージェント ページ][agents-page]では、開いているプルリクエスト (PR) に関係するすべてのエージェント タスクを確認できます。実行された操作を確認し、進行中のセッションに指示を加えることもできます。

このレッスンでは、次の作業を行います。

- エージェント ページで cloud agent のタスクを監視する。
- 実行中のセッションに追加機能を依頼する。

## シナリオ

エージェントにハイ コントラスト モードを依頼した後、チームはライト モードも追加したいと考えました。サイトのスタイルを更新し、切り替え機能を追加する作業がすでに進行中なので、同じ変更に含めるのが合理的です。エージェントに追加の指示を出し、ハイ コントラストに加えてライト モードも実装するようにします。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左ペインに表示されるタスクの一覧を確認します。割り当てたタスクには、次のようなものがあります。
   - コードベースのドキュメントを更新する。
   - ゲームを変更するアプリケーション プログラミング インターフェイス (API) を生成する。
   - Web サイトにハイ コントラスト モードを追加する。
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。次のような手順が表示される場合があります。
   - リポジトリをチェックアウトする。
   - Copilot の作業環境を作成する。
   - Model Context Protocol (MCP) サーバーをセットアップする。
   - 割り当てられたタスクを完了するための各種操作を行う。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認し、追加で監視できます。

## cloud agent に指示を加える

実行中のタスクを確認したので、エージェントがハイ コントラスト モードに取り組んでいる間に、ライト モードの切り替えも追加するよう依頼します。

1. ハイ コントラスト モードの追加に関するセッションを選択します。タイトルは Copilot が付けた名前や作業状況によって異なります。

    ![Mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セッションのセットアップが完了し、作業を開始したことを示すメッセージが表示されるまで数分待ちます。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot の作業中にセッションへ指示を加えるダイアログが表示された agents ページ](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを認識し、作業計画に反映したことを確認します。

## Copilot の作業を続ける

Copilot は更新されたタスクに取り組みます。メッセージを受け取った時点で進めていた手順が完了してから、新しい依頼を作業に反映します。

今回も完了まで数分かかるため、これまでに学んだことを振り返る時間にできます。

## まとめと次のステップ

Copilot cloud agent のタスクを監視し、指示を加えるための中心的な場所である Copilot agents ページを確認しました。Mission control では、進行中と完了済みのタスクを追跡し、作業をレビューして、進行中のタスクの範囲や追加の指示を調整できます。

このレッスンでは、次の作業を行いました。

- Mission control とエージェント ページで cloud agent のタスクを監視した。
- 実行中のセッションに追加機能を依頼した。

Copilot がアクセシビリティ機能を実装している間に、次は作成された[プルリクエストの内容を改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
