# 5-5. plan 結果を利用して terraform apply を実行する

> **前提**: この課題は [5-4. apply 前に手動承認を挟む](./5-4-environment.md) を完了していることを前提とします。

`apply` job は `plan` job とは別の job で実行されるため、`plan` job で作成した `tfplan` ファイルをそのまま参照することはできません。job をまたいでファイルを渡すには、**artifact として保存し、別の job で取得**する必要があります。

Step 4（4-7）では、その準備として `tfplan` を artifact として保存しました。この課題では、`needs` と `artifacts: true` を使ってその artifact を取得し、`terraform apply` に渡します。

また、5-4 で追加した手動承認 job を使って、`apply` の前に確実に確認を挟む構成も組み込みます。これにより、**plan した内容だけ**を apply する安全な構成を実現できます。

> 補足:
>
> このプラクティスでは学習のため `tfplan` を artifact として保存しています。実運用では、plan ファイルに機密情報や構成の詳細が含まれる可能性があるため、artifact の保持期間やアクセス権限に注意してください。

## プラクティス

`apply` job を、次の条件を満たすように実装してください。

条件は次のとおりです。

- `plan` job と同様に checkout 代わりの `before_script` と Terraform の初期化を行う
- `approve_apply` job を待機し、承認完了後に実行されるようにする
- `needs` で `plan` job の artifact を取得する
  - `artifacts: true` を使う
- `terraform apply tfplan` を実行する
  - `tfplan` は `terraform/` 配下にある前提とする

> ヒント:
>
> - GitLab では `needs` に `artifacts: true` を指定すると、前の job の artifact を利用できます
> - `terraform -chdir=terraform apply tfplan` のように `-chdir` を使うと、`terraform` ディレクトリに移動して実行できます
> - `apply` job でも `terraform init` が必要です。ジョブごとに実行環境が新しいため、`plan` job の `.terraform/` は引き継がれません

> 必要に応じて、次の公式ドキュメントを参照してください。
>
> - [GitLab CI/CD - needs](https://docs.gitlab.com/ci/yaml/#needs)
> - [Terraform apply - Passing a plan file](https://developer.hashicorp.com/terraform/cli/commands/apply#passing-a-plan-file)

## 確認

- main ブランチに push する（または UI から手動実行する）
- `plan` job が正常に完了することを確認する
- `approve_apply` job が手動承認待ちの状態で止まることを確認する
- `approve_apply` を手動で実行し、承認後に `apply` job が実行されることを確認する
  - `needs` によって `tfplan` artifact が取得されることを確認する
  - `terraform apply` が実行されることを確認する
  - apply が完了し、S3 に `BUCKET_NAME` で指定したバケットが作成されていることを確認する

---

[Step 5 トップに戻る](./README.md)
