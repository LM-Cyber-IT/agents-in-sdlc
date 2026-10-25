---
title: "演習 1 - カスタム命令 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成系人工知能 (AI) を使うときは、コンテキストが重要です。特定の方法でタスクを実行してほしい場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを参照できるようにします。[命令ファイル][instruction-files]を使うと、Copilot に作業内容だけでなく、作業方法も伝えられます。

この演習では、次の作業を行います。

- リポジトリのカスタム命令とパス単位の命令ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント規約が Copilot に適用される方法を確認する。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい規約を追加する。

> [!NOTE]
> VS Code とコマンドライン インターフェイス (CLI) の harness とは異なり、ここでは命令の変更前後で同じプロンプトを実行しません。Copilot cloud agent は GitHub Issue に対して非同期に動作するため、リアルタイムで並べて影響を確認するのが難しいためです。この harness の後半で、cloud agent が作成したプルリクエストをレビューするときに命令ファイルの影響を確認します。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。

- データ レイヤーには常に単体テストが必要です。
- ユーザー インターフェイス (UI) はダーク モードにし、現代的な印象にします。
- コードのドキュメントは TypeScript Documentation (TSDoc) コメントとして記述します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

命令ファイルを使って、Copilot がこれらの規約に沿ってタスクを実行できるようにします。

### カスタム命令

カスタム命令を使うと、コーディング スタイルや要件などのコンテキストを Copilot に提供できます。好みのコーディング規約、ライブラリ、コメントの書き方を指定できます。リポジトリ全体に適用する命令や、特定のファイルの種類に適用する命令を作成できます。

命令ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべての要求で Copilot に送信される単一の命令ファイルです。技術スタック、プロジェクトの概要、ベスト プラクティスなど、多くの要求に関連するプロジェクト レベルの情報を記述します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に適用する命令ファイルです。TypeScript や Astro などの言語や、UI コンポーネントや単体テストの作成などのタスクに関するガイドラインを記述できます。

> [!NOTE]
> 統合開発環境 (IDE) では、命令ファイルは Copilot Chat のコード生成にのみ使用され、コード補完や次の編集の提案には使用されません。
>
> Copilot Chat、Copilot コマンドライン インターフェイス (CLI)、Copilot cloud agent は、リポジトリ レベルの命令と、`applyTo` フロントマターを持つ `*.instructions.md` ファイルをコード生成時に使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準に基づく命令ファイルにも対応しています。詳細は[カスタム命令のサポート][custom-instructions-support]を参照してください。

### 命令ファイルを管理するためのベスト プラクティス

スターター プロジェクトの例を使って、命令ファイルを確認します。

- `copilot-instructions.md` には、作成するもの、プロジェクトの構成、全体的なコーディング規約など、プロジェクト レベルのガイダンスを記述します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データ レイヤーなど、ファイルの種類や特定のタスクに関するガイダンスを記述します。
- 自然な言葉で明確に説明し、適切なコードと不適切なコードの例を示します。

命令ファイルの作成方法は 1 つではありません。試しながら、プロジェクトに合う方法を見つけてください。

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

このリポジトリには、コアとなる `copilot-instructions.md` と、タスクごとの `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスも確認してください。これらは Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開きます。Astro、Drizzle データ レイヤー、テストなどの命令を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob はリポジトリのルートを基準に、命令を適用するファイルを指定します。たとえば `**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトの単体テスト作成に関する命令を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの別の命令ファイルや、プロジェクト内のファイルへのリンクがあります。これにより、大きな命令を再利用できる小さなファイルに分け、Copilot に参考例を示せます。ここに記載されているパスは、リポジトリのルートではなく命令ファイルを基準にしています。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング規約が記載されていますが、コード内ドキュメントの要件はまだありません。次の手順で TSDoc コメントとファイル ヘッダーの規則を追加します。

## リポジトリ全体の新しい規約を追加する

次に、コード内のドキュメントを TSDoc コメントとファイル レベルのコメント ヘッダーで記述するプロジェクト全体の規則を追加します。cloud agent は、この harness の後半で割り当てる Issue に取り組むときに、この規則を読み込みます。

編集を始める前に、作業用ブランチを作成します。cloud agent は Issue の対象ブランチから命令を読み込みますが、ここでは編集内容を整理してコミットします。

1. Codespace のターミナルで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. `.github/copilot-instructions.md` を再度開きます。
3. **Code formatting requirements** セクションを探します。27 行目付近にあります。コーディング規約は記載されていますが、コード内ドキュメントの規則がないことを確認します。
4. 既存の規約の直後に、次の行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. `copilot-instructions.md` を保存します。

> [!TIP]
> リポジトリ レベルの `.github/copilot-instructions.md` は全体的なガイダンスに、`*.instructions.md` は特定の言語、ファイルの種類、タスクに関するガイダンスに適しています。今回追加したドキュメント規約はプロジェクト全体に適用するため、リポジトリ レベルのファイルに記述します。

## 命令の更新をコミットし、プッシュしてマージする

cloud agent は、Issue が対象とするブランチから命令ファイルを読み込みます。次の演習では Copilot に `main` からブランチを作成させるため、命令の変更を `main` にマージしておきます。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを開き、マージします。次の演習で Issue を割り当てるとき、cloud agent がこの命令を読み込みます。

> [!TIP]
> ワークショップ中に `main` へ直接作業する場合は、ブランチの手順を省略し、`main` に直接コミットできます。ブランチの手順は、実際のプロジェクトに近い流れで進めるために含めています。

## まとめと次のステップ

Copilot がこのプロジェクトの命令ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の規約を追加しました。この harness の後半で、cloud agent が作成するプルリクエストをレビューするときに、この規約が適用された様子を確認します。

次は、最初の Issue を [Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
