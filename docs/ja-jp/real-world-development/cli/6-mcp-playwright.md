---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーでフィルター機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書く以外にも、データや外部サービスを扱い、Copilot が利用できる自動化機能を追加する必要があります。MCP サーバーを使うと、Copilot CLI に組み込まれた機能を超えるツールやサービスを利用できます。

このレッスンでは、次の作業を行います。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を確認する。
- Playwright MCP サーバーが利用できない場合は追加する。
- エージェントにブラウザーを操作させ、フィルター機能を確認する。

## シナリオ

単体テストとエンドツーエンド テストは重要ですが、ユーザー インターフェイス (UI) の変更を検証するには実際に操作する必要があります。Copilot が利用者と同じように Web サイトを操作できるようにし、変更が期待どおりに動作するかを確認します。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] を使うと、人工知能 (AI) エージェントは外部のツールやサービスと通信できます。MCP を介して最新の情報にアクセスしたり、ツールを使って開発者に代わって操作したりできます。

ツールとリソースへのアクセスは、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを介して行います。各 MCP サーバーは、エージェントが利用できるツールとリソースのセットを提供します。

よく使われる MCP サーバーの例を次に示します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を利用できます。

ほかにも多くの MCP サーバーがあります。GitHub がホストする [MCP registry][mcp-registry] で、エコシステム内のサーバーを見つけたり、貢献したりできます。

> [!CAUTION]
> MCP サーバーは、プロジェクトのほかの依存関係と同様に扱ってください。利用前にソース コードを確認し、発行元を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーが利用できない場合は追加し、Copilot が利用者と同じように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに構成されている場合は、セットアップ手順を省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類が **STDIO** であることを確認し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright を使って機能を確認する

前のレッスンでは、機能が期待どおりに動作することを手動で確認しました。次は Copilot で利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを Copilot に送信し、Playwright MCP サーバーで機能を検証します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、明示しなくても MCP サーバーの利用方法を判断します。使用するサーバーが分かっている場合は、指定すると結果が安定しやすく、トークンも節約できます。

2. Copilot がブラウザーを操作し、機能の動作を確認する手順を実行する様子を確認します。
3. Copilot の報告を読み、すべてが期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してサーバーを停止し、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認しました。このレッスンでは、次の作業を行いました。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を確認した。
- Playwright MCP サーバーが利用できない場合に追加した。
- エージェントにブラウザーを操作させ、フィルター機能を確認した。

次は、スキルとブラウザー ツールを専門的な役割にまとめる[QA カスタム エージェントを作成し][next-lesson]ます。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
