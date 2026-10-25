---
title: "演習 1 - カスタム命令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成系人工知能 (AI) を使うときは、コンテキストが重要です。Copilot に特定の方法でタスクを実行してほしい場合や、背景情報を把握してほしい場合は、その情報を参照できるようにします。コード生成のガイダンスを提供する主な方法の 1 つが[命令ファイル][instruction-files]です。

この演習では、次の作業を行います。

- リポジトリのカスタム命令とパス単位の命令ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント規約が Copilot に適用される方法を確認する。
- 現在の命令を使い、フィルター機能の最初のデータ スライス (発行元ヘルパー) を生成する。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい規約を追加する。
- 同じプロンプトを再実行し、生成されたコードが新しい規約に従う様子を確認する。
- 次の演習で Copilot が更新されたガイダンスを使えるように、命令の更新とフィルター機能の一部を `main` にコミットする。

> [!CAUTION]
> 生成されたコードが、設定した規約に従わない場合があります。Copilot は確率的に動作します。この演習では、出力が文字単位で一致することではなく、命令の更新後に動作が変わる傾向を確認します。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。

- データ レイヤーには常に単体テストが必要です。
- ユーザー インターフェイス (UI) はダーク モードにし、現代的な印象にします。
- TypeScript Documentation (TSDoc) コメントでコードを文書化します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

命令ファイルを使って、Copilot がこれらの規約に沿って作業できるようにします。

### カスタム命令

カスタム命令を使うと、コーディング スタイルや要件などのコンテキストを Copilot に提供できます。規約、ライブラリ、コード コメントの書き方などを指定し、関連性の高い提案やコード スニペットが生成されるようにします。リポジトリ全体に適用する命令や、特定のファイルに適用する命令を作成できます。

命令ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべての要求で Copilot に送信される単一の命令ファイルです。技術スタック、プロジェクトの概要、ベスト プラクティスなど、多くの要求に関係するプロジェクト レベルの情報を記述します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に適用する命令ファイルです。TypeScript や Astro などの言語や、UI コンポーネントや単体テストの作成などのタスクに関するガイドラインを記述できます。

> [!NOTE]
> 統合開発環境 (IDE) では、命令ファイルは Copilot Chat のコード生成にのみ使用され、コード補完や次の編集の提案には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの命令と、`applyTo` フロントマターを持つ `*.instructions.md` ファイルをコード生成時に使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準に基づく命令ファイルにも対応しています。詳細は[カスタム命令のサポート][custom-instructions-support]を参照してください。

### 命令ファイルを管理するためのベスト プラクティス

この会話ですべての命令ファイル作成方法を説明することはできませんが、サンプル プロジェクトの例は一般的なアプローチを示しています。

- `copilot-instructions.md` には、作成するもの、プロジェクトの構成、全体的なコーディング規約など、プロジェクト レベルのガイダンスを記述します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データ レイヤーなど、ファイルの種類や特定のタスクに関するガイダンスを記述します。
- 自然な言葉で明確に説明し、適切なコードと不適切なコードの例を示します。

命令ファイルの作成方法に唯一の正解はありません。試しながら、プロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な命令ファイルを用意してください。このプロジェクトには [UI 更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けのファイルがあります。
>
> Copilot に命令ファイルを作成させることもできます。各環境で操作方法は異なります。たとえば VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。必要な場合は、利用中の環境向けのレッスンで説明します。
>
> テンプレートや出発点を探す場合は、命令ファイル、カスタム エージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

スターター プロジェクトには、コアとなる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。Copilot への影響を変更前に確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスも確認してください。これらは Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro、Drizzle データ レイヤー、テストなどの命令があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob はリポジトリのルートを基準に、命令を適用するファイルを指定します。たとえば `**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトで単体テストを作成するための命令を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの別の命令ファイルや、プロジェクト内のファイルへのリンクがあります。これにより、大きな命令を再利用できる小さなファイルに分け、Copilot に参考例を示せます。ここに記載されているパスは、リポジトリのルートではなく命令ファイルを基準にしています。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング規約が記載されていますが、コード内ドキュメントの要件はまだありません。次の手順で TSDoc コメントとファイル ヘッダーの規則を追加します。

## 変更用ブランチを作成する

変更をブランチで管理するため、前の演習で使った Codespace に戻り、新しいブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 命令を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の作業を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なるため、必要な手順で説明します。

カスタム命令の影響を確認するため、最初に現在の命令を使ってプロンプトを送信します。その後、命令を更新して同じプロンプトを再送信し、違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に示します。
3. Chat ビューのエージェント ドロップダウンで **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、ヘルパー ファイルとテストなどのコードを更新する様子を確認します。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを受け取り、型付きの発行元配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約によるものです。
7. 提案されたコードに TSDoc コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、命令がなくても TSDoc コメントを追加する場合があります。その場合も問題ありません。命令の更新後に一貫性が向上することが重要です。

## リポジトリ全体の新しい規約を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクト レベルの情報を提供します。コーディング規約に新しいルールを追加して、コードの提案を改善します。

1. `.github/copilot-instructions.md` を再度開きます。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。コーディング規約はありますが、コード内ドキュメントの規則がないことを確認します。
3. 既存の規約の直後に、ファイル ヘッダーと TSDoc コメントを追加する次の行を挿入します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ レベルの `.github/copilot-instructions.md` は全体的なガイダンスに、`*.instructions.md` は特定の言語、ファイルの種類、タスクに関するガイダンスに適しています。今回追加したコメント規約はプロジェクト全体に適用するため、リポジトリ レベルのファイルに記述します。

## プロンプトを再実行して違いを確認する

命令の更新前後で同じプロンプトを使い、提案されるコードの違いを確認します。

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されたことを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが追加されたことを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト規約を Copilot に適用し、次の演習につながる実際のコードを更新しました。

## 命令をコミットしてブランチをプッシュする

命令ファイルもリポジトリのほかのファイルと同じように、ソース管理で管理します。変更をコミットしてリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで `git status` を実行し、命令の更新とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. 命令の更新と、ヘルパーの準備のために Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント規約の Issue に関連付けたプルリクエストを作成します。GitHub ツールを利用できる Copilot に、Issue を探して PR を作成し、Issue を解決するよう依頼します。

> [!NOTE]
> この演習では、Copilot が初めて GitHub 上で操作します。プロジェクト テンプレートには GitHub ツールがすでに接続されていますが、Copilot が最初に使用するとき、VS Code から GitHub へのサインインを求められる場合があります。画面の指示に従って許可してください。後の演習で、この接続に使われる Model Context Protocol (MCP) を確認します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** を選択します。
3. 次のプロンプトで、命令ファイルの更新に関する Issue を探し、現在のブランチからプルリクエスト (PR) を作成するよう Copilot に依頼します。命令の更新と、フィルター機能に備えて追加した発行元ヘルパーの両方を PR に記載するよう伝えます。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成する様子を確認します。
5. GitHub の操作を許可するよう求められたら、コマンドを確認し、適切な場合は **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージしてブランチを `main` に戻すよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作やシェル コマンドを許可するよう求められたら、コマンドを確認し、適切な場合は **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

Copilot が命令ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次の作業を行いました。

- 既存の命令を使ってコード生成プロンプトを送信し、Copilot の出力を確認した。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい規約を追加した。
- 同じプロンプトを再送信し、提案されたコードが新しい規約に従うことを確認した。
- 命令の更新とヘルパーの準備を `main` にコミットし、次の演習で利用できるようにした。

次は[エージェント モード][next-lesson]で命令を活用し、Copilot にコードベース全体の機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム命令][personal-instructions]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
