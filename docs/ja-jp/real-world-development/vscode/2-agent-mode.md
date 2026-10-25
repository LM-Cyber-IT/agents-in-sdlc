---
title: "演習 2 - Copilot エージェント モードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な変更でも、通常は複数のファイルを更新し、テストを実行する必要があります。開発者は必要なファイルを探し、変更を加え、テストを実行し、問題をデバッグし、更新漏れのファイルを見つけて、さらに変更します。このような作業が続きます。

ここで Copilot エージェント モードが役立ちます。

Copilot エージェント モードは、統合開発環境 (IDE) 内でより自律的に作業するように設計されています。開発者と同様に既存のプロジェクト構成を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェント モードを使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> エージェント モードと cloud agent は名前が似ていますが、用途は異なります。エージェント モードは IDE 内で作業し、短いフィードバック サイクルと対話を可能にします。cloud agent はチーム メンバーのように非同期で作業し、Issue とプルリクエストを通じてやり取りします。

この演習では、次の内容を確認します。

- Copilot エージェント モードでプロジェクトを調べ、関連ファイルを特定し、複数ファイルを連携して変更する方法。
- GitHub Copilot エージェント モードでユーザー インターフェイス (UI) とデータ レイヤーにまたがる新機能を実装する方法。
- コードベースにマージする前に、Copilot エージェント モードが生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたので、利用者がカテゴリと発行元で絞り込めるようにします。前の演習で発行元ヘルパーを追加しました。ここでは残りのデータ レイヤー、UI、テストの作業を Copilot エージェント モードで完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを見て、現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲーム一覧と詳細を表示します。単一の Astro アプリで、ビルド時にページを静的なハイパーテキスト マークアップ言語 (HTML) として生成します。ページは frontmatter 内で Drizzle オブジェクト リレーショナル マッピング (ORM) を使い、ローカルの SQLite データベースに直接クエリします。別のバックエンド アプリケーション プログラミング インターフェイス (API) やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` にあり、インタラクティブな動作には標準ドキュメント オブジェクト モデル (DOM) API を使う小規模で範囲を限定した Astro `<script>` を使用します。

### Web サイトを起動する

`npm run dev` タスクで単一の Astro 開発サーバーを起動できます。GitHub Codespace で実行します。

1. Codespace に戻ります。前の演習後に `main` にいることを確認します。
2. Codespace 内で新しいターミナルを開きます。<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
3. フィルター機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような Uniform Resource Locator (URL) を含むメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くようダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで Astro サーバーのアドレス `http://localhost:4321` を選択します。Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択します。

> [!NOTE]
> Codespace ターミナルで localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは、Web サーバーを実行している Codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、現在の機能を確認します。主な機能は次のとおりです。

- **ホームページ**: ボードゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの詳細を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の作業を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なるため、必要な手順で説明します。

現在の Web サイトは機能しますが、新しい機能を追加して改善します。テンプレートからプロジェクトを作成したとき、Issue のバックログも作成されています。GitHub Copilot に項目を表示するよう依頼します。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** エージェントは複数ファイルの変更を自律的に計画・実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルから **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップ作成時に Claude Sonnet 4.5 を使ったため、体験の一貫性を保つ目的で手順に記載しています。大規模言語モデル (LLM) は確率的に動作するため、提案内容が異なることがあります。それは正常な動作です。

> [!NOTE]
> 確率的な動作により、Copilot が別の Model Context Protocol (MCP) コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを送信して、Issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. Copilot が生成した Issue の一覧を確認します。

Copilot が利用者にとって有用と考える順に Issue を優先付けしたことを確認します。

## 命令ファイルを確認する

エージェントにコードを生成させる前に、Copilot にコンテキストを提供する命令ファイルを確認します。Web サイトへの機能追加方法を説明する [ユーザー インターフェイス (UI) 命令][ui-instructions]を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネント設計、テスト可能性、アクセシビリティの原則。
   - 次のようなファイル形式ごとの命令へのリンク。
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 命令ファイルから、ほかの命令ファイルやプロジェクト内のファイルを参照できます。パスは命令ファイルの場所を基準にします。これにより、命令を小さく再利用可能な単位に分け、例やテンプレートを示せます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の 3 つの変更が必要です。

- データ レイヤー (`src/lib/`) にフィルター ロジックを追加または改善する。
- フィルター動作のテストを追加または更新する。
- ゲーム一覧ページにフィルター UI を追加する。

コードベースにマージする前に、テストを実行して成功を確認します。Copilot エージェント モードにこれらの作業を任せます。

1. Copilot の現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、命令ファイルを追加します。

   ![UI 命令ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

4. モデルに **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関係するファイルを探す様子を確認します。データ レイヤー、UI、テストを見つけてファイルを変更し、テストを実行します。

   ![Copilot がプロジェクト ファイルを調べる画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調べ、ファイルを変更してテストを実行するため、数分かかる場合があります。作業中、エディターに **Keep** と **Undo** が表示されることがあります。完了後にはすべての変更に対して **Keep** または **Undo** が表示されます。作業中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行を確認するダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。Copilot はエラーを検出しなくなるまで、コード生成とテストを繰り返します。

   ![Copilot エージェント モードのチャット セッション完了画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの人工知能 (AI) ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いたブラウザーに戻り、新機能を確認します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot エージェント モードを使って、Tailspin Toys の Web サイトに機能を追加しました。この演習では、次の内容を確認しました。

- GitHub Copilot エージェント モードで UI とデータ レイヤーにまたがる新機能を実装する方法。
- Copilot エージェント モードでプロジェクトを調べ、関連ファイルを特定し、複数ファイルを連携して変更する方法。
- コードベースにマージする前に、Copilot エージェント モードが生成した変更とテストをレビューする方法。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習 — ページネーションを実装する

ゲーム一覧が増えるにつれて、ページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションの実装を依頼してみてください。次の点を検討します。

- 既存の命令ファイルなど、既存のベスト プラクティスに従う。
- ページ サイズを利用者が選べるようにするか、固定するかを検討する。
- 希望する実装に必要なガイダンスをプロンプトに含める。
- Copilot に変更を依頼し、コンテキストを提供しながら反復する。これは Copilot を使う通常の作業方法です。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
