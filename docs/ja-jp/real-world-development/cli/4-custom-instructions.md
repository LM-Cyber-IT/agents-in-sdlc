---
title: "レッスン 4 - カスタム命令で Copilot を導く"
description: "リポジトリの命令を確認し、ドキュメント作成の規約を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成系人工知能 (AI) を使うときは、コンテキストが重要です。特定の方法でタスクを実行してほしい場合は、その指針を Copilot が参照できるようにします。[命令ファイル][instruction-files]には、必要なコードだけでなく、コードの構成方法も記述します。フィルター機能を構築したので、Copilot が参照した命令を確認し、ドキュメント作成の規約を追加してコードに適用します。

このレッスンでは、次の作業を行います。

- リポジトリの命令とパス単位の命令がエージェントに適用される方法を確認する。
- コーディング規約を守るため、命令ファイルを更新する。
- 命令ファイルがコードに与える影響を確認する。

## シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。

- コメントにはコードの言い換えではなく、意図や判断が自明でない理由を記述する。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を TypeScript Documentation (TSDoc)/JavaScript Documentation (JSDoc) で記述する。該当する場合、注入可能な `db` 引数も含める。
- 再利用可能な Astro コンポーネントでは `Props` の契約を記述する。関連コードの変更時にはコメントも更新する。
- 既存のフォーマットと lint のガイダンスを維持する。

命令ファイルを使って、Copilot がこれらの開発規約に沿って作業できるようにします。

## 命令ファイル

カスタム命令は、コーディング スタイルや要件などのコンテキストを Copilot に提供します。規約、ライブラリ、コード コメントの書き方などを指定できます。リポジトリ全体に適用する命令や、特定のファイルやタスクに適用する命令を作成できます。

命令ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべての要求で Copilot に送信される単一の命令ファイルです。多くの要求に関係するプロジェクト レベルの情報を記述します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスク向けのガイドラインを記述します。

> [!NOTE]
> ほかの命令形式への対応状況は環境によって異なります。特定の形式を利用する前に、[カスタム命令のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## プロジェクトのカスタム命令ファイルを確認する

スターター プロジェクトには命令ファイルが用意されています。変更の影響を確認する前に、既存の内容を調べます。

1. Codespace に戻ります。
2. ターミナルではなく、Codespaces エディターで `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。この命令は、リポジトリ内での Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro、Drizzle データ レイヤー、テストなどの命令があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob で命令が適用されるファイルを指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの命令ファイルやプロジェクト ファイルへのリンクを確認します。これにより、大きな命令を再利用できる小さなファイルに分け、Copilot に参考例を示せます。

## チームのガイダンスに合わせて命令ファイルを更新する

既存ファイルには改善の余地があります。コアとなる `copilot-instructions.md` を更新し、新しく生成する TypeScript に TSDoc コメントが追加されるようにします。

1. `.github/copilot-instructions.md` で **Code formatting guidance** セクションを探します。35 行目付近にあります。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話の開始時にリポジトリの命令を読み込みます。フィルター機能のコンテキストを維持したまま新しいガイダンスを読み込むため、会話を終了して再開します。

1. フィルターの会話が開いている場合は `/exit` を入力して終了します。Copilot CLI は会話を自動保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたらフィルターの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に次のプロンプトを送信して、更新したガイダンスを適用します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して、変更された TypeScript ファイルを確認します。新しい TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が命令ファイルのコンテキストを取得する方法を確認し、新しい規約を機能に適用しました。このレッスンでは、次の作業を行いました。

- リポジトリの命令とパス単位の命令がエージェントに適用される方法を確認した。
- コーディング規約を守るため、命令ファイルを更新した。
- 命令ファイルがコードに与える影響を確認した。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストを一貫して実行できるようにします。

## リソース

- [Copilot CLI にカスタム命令を追加する][instruction-files]
- [カスタム命令のサポート][custom-instructions-support]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
