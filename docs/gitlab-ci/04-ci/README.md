# Step 4: 実践編（CI）

このステップでは、Step 2・Step 3 で学んだ GitLab CI/CD の知識を活かして、Terraform の CI を構築します。

CI（継続的インテグレーション）では、コードの変更を取り込む前に自動で検証を実行し、問題を早い段階で検出します。Terraform では `fmt`・`validate`・`plan` を自動化することで、誤った変更が main ブランチに入るリスクを下げられます。

このステップでは、次の要素を扱います。

- Terraform を CI から実行する設定
- `terraform fmt` でフォーマット確認
- `terraform init` / `terraform validate` で構成検証
- `terraform plan` で変更内容を事前確認
- Merge Request をきっかけに CI を実行する設定
- plan 結果を artifact として保存する設定

> 進め方:
>
> 4-2 で `ci/terraform.gitlab-ci.yml` を新規に作成し、4-3 以降はその pipeline 定義を編集しながら進めます。
> `.gitlab-ci.yml` には `include` を設定して、このファイルを取り込みます。

このプラクティスでは local backend を使用します。
S3 backend などの remote backend は扱わず、CI 上で `fmt` / `validate` / `plan` を実行する流れを確認することに集中します。

## プラクティス一覧

| #   | タイトル                                                           |
| --- | ------------------------------------------------------------------ |
| 4-1 | [Terraform コードを準備する](./4-1-setup-terraform.md)             |
| 4-2 | [CI から Terraform を実行できるようにする](./4-2-run-terraform.md) |
| 4-3 | [terraform fmt でフォーマットを確認する](./4-3-fmt.md)             |
| 4-4 | [terraform init / validate で構成を検証する](./4-4-validate.md)    |
| 4-5 | [terraform plan で変更内容を確認する](./4-5-plan.md)               |
| 4-6 | [Merge Request で CI を実行する](./4-6-mr-trigger.md)              |
| 4-7 | [plan 結果を artifact として保存する](./4-7-artifact.md)           |
