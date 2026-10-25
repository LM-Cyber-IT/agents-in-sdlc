---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせて、専門的な人工知能 (AI) アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルでエージェントを定義し、特定の作業を効果的に進めるための指示、ベスト プラクティス、コーディング パターン、分野固有の知識を提供します。たとえば、[Web Content Accessibility Guidelines (WCAG)][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディングを確認するセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントなどに、チームの知見を反映できます。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダー内の Markdown ファイル、またはグローバルな `~/.copilot/agents` に定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門性、指示を定義する Markdown プロンプトを記述します。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。簡単に区別するなら、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、作業中にスキルや別のエージェントを調整できます。このラボのアクセシビリティ カスタム エージェントは、アクセシビリティ ガイドラインに照らしてサイトをレビューし、更新します。その作業の一部として、プルリクエスト ワークフローのスキルやテストを実行・管理するスキルを呼び出せます。

> [!NOTE]
> カスタム エージェントの作成方法に唯一の正解はありません。AI を使うほかの場面と同じように、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者がクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近の利用者のフィードバックでは、テキストと背景のコントラストが不十分なため、現在のダーク テーマが読みにくいという意見がありました。このアクセシビリティ上の問題に対応するため、デザイン チームは利用者が切り替えられるハイ コントラスト モードを求めています。

この演習では、次の作業を行います。

- 既存のアクセシビリティ カスタム エージェントを確認する。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装する。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ向けのカスタム エージェントはすでに作成されています。内容を確認し、Copilot を導く方法を把握します。

アクセシビリティの変更をフィルター PR と分けるため、Codespace に戻り、`main` から新しいブランチを作成します。

1. ターミナルで次のコマンドを実行します。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. `.github/agents/accessibility.md` を開きます。
3. YAML フロントマターの `name` と `description` を確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含むフロントマターが必要です。

4. 続くセクションを読み、次の内容を確認します。
   - アクセシビリティに配慮した Web サイトのコードを生成する際の主要な役割。
   - アクセシビリティのベスト プラクティス。
   - ハイパーテキスト マークアップ言語 (HTML)、カスケーディング スタイル シート (CSS)、JavaScript のコード例。
   - よくある問題や間違い。

## Copilot Chat でカスタム エージェントを使う

VS Code では、`.github/agents` に定義されたカスタム エージェントが Copilot Chat ビュー下部のエージェント ドロップダウンに表示されます。カスタム エージェントを選ぶと、その命令とツールを使うチャット セッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の作業を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なるため、必要な手順で説明します。

1. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていない場合は選択します。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント ドロップダウンを選択します。現在のエージェントが表示されます。既定値は **default** です。
3. 利用可能なエージェントから **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べ、変更を提案して、プロジェクト ファイルを編集する様子を確認します。編集内容は、ファイル パスと差分とともにチャット内に表示されます。
6. 編集が進むと、エージェントが変更した作業ファイルを示す **Files changed** の表示が更新されます。

> [!NOTE]
> 作業には数分かかる場合があります。Copilot はリポジトリを実際に変更し、Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集します。

次の演習では、進行中の作業をレビューし、エージェントに追加の指示を出します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]について確認しました。カスタム エージェントは、特定のタスクや分野に合わせた専門的な AI アシスタントです。チームの知見や規約を再利用可能なエージェントに反映し、特定の作業を効果的に進めるよう Copilot を導けます。

この演習では、次の内容を確認しました。

- カスタム エージェントの定義方法。
- Copilot Chat のエージェント モードでカスタム エージェントを使う方法。

次は[エージェントの作業を監視して指示を加え][next-lesson]、変更を確認しながら同じセッションにライト モードの切り替えを追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
