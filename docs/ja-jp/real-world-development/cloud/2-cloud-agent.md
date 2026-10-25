---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム命令][previous-lesson] |
|:--|

多くの組織が技術的負債を抱えています。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot cloud agent は、コードの更新や機能追加などを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフトのプルリクエスト (PR) を作成します。これにより、時間のかかる作業を任せ、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次の内容を確認します。

- コード生成環境をカスタマイズする。
- 操作を安全に実行する。
- 範囲を明確にした Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には、対処したい技術的負債があります。サイトの初期バージョンを開発した委託先が、ドキュメントをほとんど作成しませんでした。まず、アプリケーション内のエクスポートされたすべての関数に TypeScript Documentation (TSDoc) コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。ゲーム詳細ページに同じカテゴリの関連ゲームを表示し、利用者がほかのゲームを続けて見られるようにします。完成度の高い実装は必要なく、ユーザー エクスペリエンス (UX) の受け入れテストに使えるものが必要です。この作業は現在のブロッカーですが、優先度の高い Issue がほかにもあります。

こうした作業は優先順位が下がりがちなので、Copilot cloud agent に任せるのに適しています。非同期で作業している間、開発者は別の作業に集中し、後から結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent][copilot-agents] は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる][assign-issue]方法もあります。割り当てると、Copilot は進捗を追跡するドラフトのプルリクエスト (PR) を作成し、環境を準備してタスクに取りかかります。作業中または完了後に Copilot のセッションを確認できます。提案内容のレビュー準備が整うと、プルリクエストで開発者に通知します。

## 適切な範囲の指示が重要

GitHub Copilot に任せても、考える必要がなくなるわけではありません。一見単純な操作にも複雑な要素があります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に設定してください。人工知能 (AI) ペア プログラマーとして段階的に作業し、試しながら学び、必要に応じて調整します。生成系人工知能を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

これまでの演習では、カスタム命令と Copilot への適用方法を確認しました。カスタム命令を実際に扱っていれば `.github/copilot-instructions.md` の動作を確認済みです。まだの場合は、この機会に見ておきましょう。cloud agent に作業を割り当てる前に、このリポジトリに含まれる命令ファイルを読み取り専用で確認し、後で影響を確認できるようにします。

リポジトリの GitHub Web UI、または起動中の Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` の **Code standards** セクションを確認します。特に TypeScript の規約と TSDoc コメントの要件に注目してください。
- `.github/instructions/unit-tests.instructions.md` の `applyTo` フロントマターを確認します。この命令は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、作成されたプルリクエストで TSDoc コメント、TypeScript の規約、コメント ヘッダーを確認します。これらは命令ファイルから適用されます。後の演習で PR をレビューするときにも確認します。

## Copilot cloud agent の開発環境を準備する

コードを作成するときは、通常、環境の準備とスクリプトの実行が必要です。Copilot にタスクを割り当てる場合も同様です。Copilot はソフトウェア エンジニアのように作業します。

cloud agent は、作業環境に [GitHub Actions][github-actions] を使います。作業開始前に必要な開発ツールや依存関係を用意するには、`.github/workflows/copilot-setup-steps.yml` で構成する[セットアップ ワークフロー][setup-workflow]を作成します。このラボでは演習を進めやすくするため、事前に設定済みです。単一の Astro アプリ用に、Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションとシードが済んだ SQLite データベースを利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 各手順に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが使用されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。ここでクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL になりました)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストに必要なデータを用意するため、ローカルの SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

このファイルは通常の GitHub ワークフローと同様ですが、いくつかの点に注目してください。

- `copilot-setup-steps` という 1 つのジョブがあります。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot が実行するまで待たずに、ワークフローをテストする場合に便利です。

## ドキュメントを追加する

多くのプロジェクトでは、ドキュメントが古いか、まったくありません。これは対処されないままになりやすい技術的負債で、作業効率を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメント作成を支援できるため、この作業は cloud agent に割り当てるのに適しています。バックグラウンドで必要なドキュメントを作成し、後の演習で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく生成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` などになります。PR が表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで、数分かかる場合があります。Copilot は環境を準備し (前述のワークフローを実行)、プロジェクトを分析して、作業方法を決めています。

13. todo リストを確認し、Copilot が実行するタスクを把握します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを確認します。
15. **View session** ボタンを選択します。

    ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示を更新しないと最新の状態が反映されない場合があります。

16. ライブ セッションをスクロールして、Copilot が問題を解決する様子を確認します。コードを調べて状態を把握し、計画を考えて決定し、コードを作成します。

この作業には数分かかる場合があります。Copilot cloud agent の主な利点は、非同期でタスクを実行し、開発者が別の作業に集中できることです。この利点を活かすため、もう 1 つのタスクも cloud agent に割り当ててから、アプリケーションの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

Copilot cloud agent には、作業を分担できる利点があります。デザイン チーム向けの関連ゲーム セクションの追加に時間がかからないとしても、その間に別のタスクを進められます。cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. 空のテンプレートを使うため **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

少しすると、Issue の最初のコメントに 👀 が表示され、Copilot が作業中であることが分かります。

![Copilot が Issue の作業中であることを目の絵文字で示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェア エンジニアと同様に動作するため、完了まで常に監視する必要はありません。完了後に結果をレビューします。次はカスタム エージェントの作成と利用に進みます。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]を確認しました。cloud agent に Issue を割り当てると、非同期で作業できます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などに利用できます。

この演習では、次の内容を確認しました。

- コード生成環境をカスタマイズする。
- 操作を安全に実行する。
- 範囲を明確にした Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

cloud agent がバックグラウンドで作業している間に、カスタム エージェントの作成と利用に進みます。[Copilot cloud agent は Model Context Protocol (MCP) サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム命令にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
