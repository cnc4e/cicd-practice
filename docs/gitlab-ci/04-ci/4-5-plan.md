# 4-5. terraform plan で変更内容を確認する

> **前提**: この課題は [4-4. terraform init / validate で構成を検証する](./4-4-validate.md) を完了していることを前提とします。

`terraform plan` は、現在のコードを適用した場合にどのような変更が行われるかを事前に確認するコマンドです。
実際のリソース作成は行わず、レビュー前に差分を確認できます。

## プラクティス

`ci/terraform.gitlab-ci.yml` を編集し、次の条件を満たしてください。

条件は次のとおりです。

- `terraform plan` を実行する
- `bucket_name` 変数を `-var` で渡す（値は CI/CD Variable の `BUCKET_NAME` から参照する）
- `BUCKET_NAME` の値は `日付-作業者名-cicd-practice` 形式にする（例: `20240101-yamada-cicd-practice`）
- plan 結果を `-out=tfplan` でファイルに保存する
- 実行順は `fmt → init → validate → plan` にする

> ヒント:
>
> - `terraform -chdir=terraform plan -var="bucket_name=$BUCKET_NAME" -out=tfplan` のように指定できます
> - `BUCKET_NAME` は GitLab の CI/CD Variables で事前に設定してください

必要に応じて、次の公式ドキュメントを参照してください。

- [terraform plan](https://developer.hashicorp.com/terraform/cli/commands/plan)

## 確認

- 変更を push し、`terraform plan` が成功することを確認する
- ログに作成予定のリソース（S3 バケット）が表示されることを確認する
- `terraform/main.tf` に存在しない引数（例: `invalid_arg = "test"`）を追加して push し、`plan` が失敗することを確認する
- 確認後、追加した引数を元に戻して push する

---

次のプラクティス：[4-6. Merge Request で CI を実行する](./4-6-mr-trigger.md)
