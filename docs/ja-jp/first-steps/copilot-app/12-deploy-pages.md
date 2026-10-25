---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app を使って Pages ワークフローを構成し、プルリクエストをレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages を使って Space Quiz を公開しましょう。Copilot はワークフローの準備やデプロイを支援しますが、変更をレビューし、サイトが公開されたことを確認するのは開発者の役割です。

このレッスンでは、次の内容を学習します。

- Copilot に GitHub Pages のデプロイを構成するよう依頼する。
- マージする前にワークフローとプルリクエストをレビューする。
- デプロイの状況を確認し、公開されたサイトを検証する。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、利用者が簡単に試せる方法はまだありません。単一ファイルのサイトを GitHub Pages で公開すると、サーバーやランタイム依存関係を追加せずに公開 URL から利用できるようになります。

## Copilot にサイトのデプロイを依頼する

GitHub Pages の構成を依頼するため、Copilot app の `space-quiz` セッションを使います。

1. `space-quiz` の Copilot app セッションを開きます。
2. 次のプロンプトを送信します。

   ```plaintext
   Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
   ```

![マージ済みの space-quiz プルリクエスト、Build website・Upload artifact・Deploy to Pages の各ステップが成功した GitHub Actions の Deploy website 実行、yourname.github.io/space-quiz/ に公開された Space Quiz の 3 段階の図。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> このプロンプトは、エージェントにプルリクエストの作成とマージ、およびリポジトリの Pages 設定の変更を依頼します。先に進む前に、差分、デプロイの権限、GitHub の承認要求を確認してください。

## ワークフローをレビューしてデプロイを確認する

ワークフローと公開結果を確認し、エージェントの報告だけに頼らず、サイトが実際に利用できることを確かめます。

1. リポジトリに README がまだない場合に限り、README が追加されていることを確認します。
2. ワークフローのビルド手順とデプロイ手順を含め、プルリクエストの変更をレビューします。
3. 必須のプルリクエスト チェックが成功した後にのみマージします。
4. GitHub Actions のデプロイ実行を追跡し、成功するまで確認します。
5. 報告された URL を開き、Space Quiz が読み込まれることを確認します。

エージェントが Pages の有効化やプルリクエストのマージを行えない場合は、示された手順に従ってください。デプロイが成功し、公開サイトが読み込まれるまで完了とはみなしません。

## まとめと次のステップ

Space Quiz を公開し、エージェントの報告だけに頼らずデプロイを確認しました。続けて[レッスン 13: 振り返りと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
