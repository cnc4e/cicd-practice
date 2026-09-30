# Step 6: Runner 編

これまでのステップでは、GitLab が用意した環境で pipeline が実行されてきました。このステップでは、job の実行基盤である **Runner** に焦点を当てます。

Runner には GitLab が管理する **GitLab-hosted runner** と、自分で用意する **Self-managed runner** の 2 種類があります。このステップでは、それぞれの特徴を理解したうえで、Self-managed runner を実際に導入して pipeline を実行します。

このステップでは、次のような要素を扱います。

- **Runner の概念**：job がどこで実行されるかを理解します
- **GitLab-hosted runner**：GitLab が提供する runner の環境を実際に確認します
- **Self-managed runner**：自分で用意した環境を runner として登録し、pipeline を実行します

> **注意**: Self-managed runner は public プロジェクトへの登録が推奨されません。  
> 6-3 以降の課題に進む前に、Step 1 で作成したプロジェクトを **Private に変更**してください。変更方法は 6-3 で案内します。

## プラクティス一覧

| #   | タイトル                                                                |
| --- | ----------------------------------------------------------------------- |
| 6-1 | [Runner とは何か](./6-1-runner.md)                                      |
| 6-2 | [GitLab-hosted runner で環境を確認する](./6-2-gitlab-hosted.md)         |
| 6-3 | [Self-managed runner とは何か](./6-3-self-managed-overview.md)          |
| 6-4 | [Self-managed runner を導入する](./6-4-self-managed-setup.md)           |
| 6-5 | [Self-managed runner で pipeline を実行する](./6-5-self-managed-run.md) |
