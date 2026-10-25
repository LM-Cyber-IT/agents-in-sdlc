---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせて、専門的な人工知能 (AI) アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルでエージェントを定義し、特定の作業を効果的に進めるための指示、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に提供します。たとえば、[Web Content Accessibility Guidelines (WCAG)][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディングを確認するセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントなどに、チームの知見を反映できます。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダー内の Markdown ファイル、またはグローバルな `~/.copilot/agents` に定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門性、指示を定義する Markdown プロンプトを記述します。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。簡単に区別するなら、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、作業中にスキルや別のエージェントを調整できます。このラボのアクセシビリティ カスタム エージェントは、アクセシビリティ ガイドラインに照らしてサイトをレビューし、更新します。その作業の一部として、プルリクエスト ワークフローのスキルやテストを実行・管理するスキルを呼び出せます。

> [!NOTE]
> カスタム エージェントの作成方法に唯一の正解はありません。AI を使うほかの場面と同じように、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、カスタム エージェントを使って次の内容を確認します。

- カスタム エージェントの定義方法。
- タスクをカスタム エージェントに割り当てる方法。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者がクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近の利用者のフィードバックでは、テキストと背景のコントラストが不十分なため、現在のダーク テーマが読みにくいという意見がありました。このアクセシビリティ上の問題に対応するため、デザイン チームは利用者が切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なので、カスタム エージェントを使って、できるだけ早く機能を作成します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ向けのカスタム エージェントはすでに作成されています。内容を確認し、Copilot を導く方法を把握します。

1. Codespace に戻り、アクセシビリティ カスタム エージェントのファイル `.github/agents/accessibility.md` を開きます。
2. YAML フロントマターに `name` と `description` があることを確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシビリティに配慮した Web サイトのコードを生成する際の主要な役割。
   - アクセシビリティのベスト プラクティス。
   - ハイパーテキスト マークアップ言語 (HTML)、カスケーディング スタイル シート (CSS)、JavaScript のコード例。
   - よくある問題や間違い。

## Issue を作成して割り当てる

Mission control は、環境内のエージェントを管理するための中心的な場所です。Copilot cloud agent にタスクを割り当て、進行状況を監視し、方向転換や追加の指示を行えます。まず、ハイ コントラスト モードの作成タスクを Copilot に割り当てます。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. カスタム エージェントの一覧から **Accessibility agent** を選択します。

   ![カスタム エージェントとアクセシビリティ エージェントが選択された cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドでタスクを開始したことを確認します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]について確認しました。カスタム エージェントは、特定のタスクや分野に合わせた専門的な AI アシスタントです。チームの知見や規約を再利用可能なエージェントに反映し、特定の作業を効果的に進めるよう Copilot を導けます。

この演習では、次の内容を確認しました。

- カスタム エージェントの定義方法。
- タスクをカスタム エージェントに割り当てる方法。

Copilot がハイ コントラスト モードを実装している間に、次は Mission control から[エージェント セッションを監視して指示を加えます][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [Organization でカスタム エージェントを使う準備][org-custom-agents]
- [Enterprise でカスタム エージェントを使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
