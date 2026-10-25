---
title: "演習 5: GitHub Copilot の作業を改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善と機能追加を目的に、GitHub Copilot と複数のタスクに取り組みました。コードへのドキュメント追加、デザイン チームが検討する関連ゲーム機能の作成、ハイ コントラストとライト モードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って改善します。

### シナリオ

生成系人工知能 (AI) を導入しても、ソフトウェア設計と開発・運用 (DevOps) の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスを適用します。ここでは、チームのレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に対する GitHub Copilot の提案を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、直接監督されずにタスクを実行するため、安全性を確保するためのセキュリティ上の制約があります。

- Copilot はリポジトリに対する読み取りアクセス権を持ち、書き込みアクセス権はコード作業用のブランチに限られます。
- cloud agent は GitHub Actions 内で実行され、作業用に別の一時的な環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- Model Context Protocol (MCP) サーバーを含む[外部リソースへのアクセスは、既定で制限されています][agent-firewall]。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、コードへのドキュメント追加を確認します。GitHub.com の標準 PR インターフェイスを使います。

> [!NOTE]
> PR には、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。既定では MCP サーバーへの呼び出しを含め、Copilot の外部リソースへのアクセスが制限されているため、これは想定内です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、PR の一覧を開きます。
3. **Add missing documentation** など、コードへのドキュメント追加に関するタイトルの PR を開きます。

> [!NOTE]
> Copilot が作業中の場合、PR のタイトルに **[WIP]** が含まれます。その場合は、Copilot の作業が完了するまで待ちます。数分かかる場合があるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら、**Files changed** タブを選択して変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc コメントなど、更新されたコードとドキュメントを確認します。実際の変更内容は異なる場合があります。

   TSDoc コメント、TypeScript の規約、コメント ヘッダーを確認してください。これらは演習 2 の冒頭で確認したカスタム命令ファイルに由来します。

6. 更新内容を確認したら、**Conversation** タブに戻り、下へスクロールします。
7. ワークフローの承認待ちを示す表示があることを確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが追加されたことを確認します。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

PR での Copilot との作業は一方通行ではありません。チームのメンバーと同じように、PR のコメントやコードのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応する新しいセッションを開始します。生成結果は決定論的ではないため、依頼文を一律には指定できません。たとえば、次のような更新を依頼できます。

- 各コード ファイルの先頭に、ファイルの役割を短く説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TypeScript Documentation (TSDoc) コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例など、コードベースに追加したいドキュメントの変更を依頼するコメントを追加し、ほかのユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択して、Copilot の作業を確認します。更新のために新しいセッションが開始されます。
3. **Back to pull request** を選択すると、PR に戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了したら、PR に新しいコミットが追加されていることを確認します。
5. **Files changed** タブを選択して、変更をレビューします。

納得できるまで改善を続けてください。完了したら、PR をドラフトからレビュー可能な状態に変更し、`main` ブランチにマージできます。

![PR をレビュー可能な状態に変更する画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

次に、ゲーム詳細ページに関連ゲームを表示する Issue に対して Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、関連ゲーム機能に関するタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容を確認したら、**Conversation** タブに戻り、下へスクロールします。
6. ワークフローの承認待ちを示す表示があることを確認します。
7. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションにワークフローが追加されたことを確認します。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace を開き、ターミナルで次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名 (例: `copilot/fix-8`) に置き換えます。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。これまでと同じように、cloud agent に更新を依頼できます。たとえば、表示する関連ゲームの件数を調整したり、コメント ヘッダーと TSDoc コメントを追加したりできます。ただし、この Issue はカスタム命令を更新する**前に**割り当てた点に注意してください。これまでと同じように、**Conversation** タブで新しいコメントを追加すると、Copilot が内容を確認してセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントで実装した機能を確認します。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 の Mission control で追加依頼したライト モードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、ハイ コントラスト モードに関するタイトルの PR を選択します。

> [!NOTE]
> Copilot が作業中の場合、PR のタイトルに **[WIP]** が含まれます。その場合は、Copilot の作業が完了するまで数分待ちます。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 特に次の実装を確認します。
   - 表示モードを切り替えるユーザー インターフェイス (UI) コンポーネント。
   - 利用者の設定を保存する local storage の使用。
   - ハイ コントラスト モードとライト モードのカスケーディング スタイル シート (CSS) またはスタイル変更。
   - Accessible Rich Internet Applications (ARIA) ラベルやキーボード操作などのアクセシビリティ属性。
   - モードの切り替えを管理する JavaScript/TypeScript コード。
6. 更新内容を確認したら、**Conversation** タブに戻り、下へスクロールします。
7. ワークフローの承認待ちを示す表示があることを確認します。
8. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが追加されたことを確認します。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace を開き、ターミナルで次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名に置き換えます。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイ コントラストとライト モードの切り替えを試します。ページを再読み込みした後も設定が維持されることを確認してください。

アクセシビリティ カスタム エージェントがアクセシビリティのベスト プラクティスに沿って機能を実装するよう Copilot を導いたことを確認します。改善点や懸念があれば、前の PR と同じようにコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 追加の作業を委任する

cloud agent に実際のバックログ項目を任せれば、その間に別の作業を進められます。リポジトリにいくつか Issue を登録し、Copilot に割り当ててみてください。たとえば、次のような作業があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent harness を完了しました。これらのレッスンを通して、次の作業を行いました。

- このリポジトリに含まれるカスタム命令ファイルを確認し、後で cloud agent の出力への影響を確かめられるようにした。
- Issue を Copilot cloud agent に割り当て、環境の準備、計画、非同期の実行を確認した。
- アクセシビリティ用のカスタム エージェントを作成して使い、ハイ コントラストとライト モードの切り替えを追加した。
- Copilot agents ページを Mission control として使い、アクセシビリティ セッションを監視して指示を加えた。
- **@copilot** に変更を依頼し、ワークフローを承認して、cloud agent のプルリクエストをレビュー・改善した。

## レビューと次のステップ

Cloud agent harness を完了しました。引き続き学ぶ場合は、ほかの harness で今回の内容を補完できます。

- **[VS Code harness](../../vscode/)** — IDE で Copilot Chat のエージェント モードと MCP 統合を確認します。
- **[CLI harness](../../cli/)** — Plan と Autopilot、エージェント スキル、カスタム品質保証 (QA) エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使い、ターミナルから 2 つの変更をレビューしてリリースします。

自分のリポジトリでは、次のような方法でさらに試せます。

- リファクタリングやテスト範囲の Issue を cloud agent に割り当てる。
- ほかの分野のカスタム エージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
