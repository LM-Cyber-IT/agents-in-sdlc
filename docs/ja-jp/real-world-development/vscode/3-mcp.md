---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

エージェント モードでフィルター機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続して、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開してプルリクエスト (PR) を開きます。

この演習では、次の作業を行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーが新しいツールで Copilot を拡張する方法を確認する。
- Playwright MCP サーバーをワークスペースに追加する。
- Copilot にサーバーを使わせ、ブラウザーでフィルター機能を手動テストする。
- ブランチを公開し、フィルター機能のプルリクエストを開く。

## Model Context Protocol (MCP) とは

エージェント モードは、エディターの外部に接続できると、さらに便利になります。Model Context Protocol (MCP) を使うと、エージェントは外部のツールやサービスと通信できます。

![コンテキスト、大規模言語モデル (LLM)、ツールと連携するエージェント モードの内部構造、および MCP サーバーと VS Code 拡張機能が提供するツールを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、人工知能 (AI) エージェントは外部のツールやサービスと通信できます。MCP により、リソースを使って最新情報にアクセスしたり、ツールを使って開発者に代わって操作したりできます。

これらのツールとリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを介してアクセスします。MCP サーバーは、AI エージェントと外部ツール (既存のアプリケーション プログラミング インターフェイス (API) や npm パッケージなどのローカル ツール) の通信を管理します。各 MCP サーバーは、エージェントが利用できる異なるツールとリソースを提供します。

よく使われる MCP サーバーの例を次に示します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリの管理 API を利用できます。リポジトリの作成や更新、Issue やプルリクエストの管理などをエージェントが実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などをエージェントが実行できます。

ほかにも多くの MCP サーバーがあります。GitHub がホストする [MCP registry](https://github.com/mcp) で、エコシステム内のサーバーを見つけたり、貢献したりできます。

> [!CAUTION]
> MCP サーバーは、プロジェクトのほかの依存関係と同様に扱ってください。利用前にソース コードを確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセス許可には注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルでは、この VS Code ワークスペースで利用できる MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` サーバーが構成されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクト テンプレートに含まれているため、前の演習で Copilot が Issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code が HTTP 経由で接続し、Copilot が初めてツールを使用するときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot がブラウザーを操作できるため、機能をテストできます。

1. `.vscode/mcp.json` の `github` と同じ階層に `playwright` エントリを追加し、ファイルが次のようになるようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル** サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを使うと、Playwright は画面を表示せずにブラウザーを実行します。デスクトップ画面のない Codespace では、これが必要です。

> [!NOTE]
> Tailspin Toys プロジェクトではエンドツーエンド テストに Playwright を使うため、必要なブラウザーは通常すでにインストールされています。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行して再試行するよう依頼します。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。Copilot がサーバーのツールを初めて使うときにサーバーを起動し、信頼するかどうかを確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** 操作が表示されます。これを選択してサーバーを起動し、設定が正しいことを確認します。
2. VS Code がサーバーを信頼するか確認した場合は、構成を確認して、サーバーを起動できるよう信頼を選択します。

手動での起動は任意です。省略した場合は、次の手順で Copilot がツールを初めて必要としたときに VS Code がサーバーを起動し、信頼の確認を求めます。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。開発者がアプリを操作する代わりに、エージェントがページを開き、移動してフィルターを適用し、結果を読み取って要約します。会話から離れずに機能の動作を確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を参照します。エージェントは支援技術と同様に、ボタン、リンク、リスト項目など、ラベル付きの構造化された要素を処理します。そのため、簡単な機能テストをアクセシビリティの基本的な確認にも活用できます。

サーバーを接続し、アプリを起動した状態で、次のプロンプトを Copilot に送信します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーからブラウザーを起動し、各手順を実行して結果を報告します。報告を Issue の受け入れ基準と照らし合わせてください。問題があれば、プルリクエストを作成する前に追加の質問をしたり、コードの修正を依頼したりします。

> [!NOTE]
> このテストには、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送信する前に再起動します。Playwright MCP サーバーの初回使用時にブラウザーのダウンロードが必要な場合があります。不足を報告されたら、`npx playwright install chromium` を実行して再試行するよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したので、チームがレビューできるようプルリクエスト (PR) を開きます。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルを開き、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![変更が表示された Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してリポジトリにブランチをプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンド ライン インターフェイス (CLI) など、プルリクエストを作成する方法はいくつかあります。ここでは GitHub Copilot に PR を作成してもらい、関連する Issue と紐付けます。

1. Copilot Chat パネルで **New Chat** を選択し、新しいセッションを開始します。
2. Copilot が GitHub ツールを使えるよう、エージェント ドロップダウンで **Agent** が選択されていることを確認します。
3. 次のプロンプトを送信して PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報の収集や操作を Copilot に許可します。GitHub ツールの初回使用時に GitHub 認証が求められる場合は、画面の指示に従います。
5. Copilot が Issue を検索し、適切なものを見つけて PR を作成したことを確認します。
6. Copilot が生成したリンクを選択して PR をレビューします。ただし、まだマージしないでください。

## まとめと次のステップ

この演習では、次の作業を行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーが新しいツールで Copilot を拡張する方法を確認した。
- Playwright MCP サーバーをワークスペースに追加した。
- Copilot にブラウザーでフィルター機能を手動テストさせた。
- ブランチを公開し、フィルター機能のプルリクエストを開いた。

MCP サーバーを使って機能を**テスト**しました。MCP は Copilot を拡張する方法の 1 つです。次は[カスタム エージェントを作成し][next-lesson]、アクセシビリティ レビューなどの専門的な作業を効率化します。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
