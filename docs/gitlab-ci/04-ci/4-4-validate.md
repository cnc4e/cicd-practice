# 4-4. terraform init / validate で構成を検証する

> **前提**: この課題は [4-3. terraform fmt でフォーマットを確認する](./4-3-fmt.md) を完了していることを前提とします。

`terraform init` は provider のダウンロードや backend 初期化を行うコマンドです。
`terraform validate` は構文と設定の整合性を確認するコマンドです。

`validate` は `init` の後で実行する必要があります。

## プラクティス

`ci/terraform.gitlab-ci.yml` を編集し、次の条件を満たしてください。

条件は次のとおりです。

- `terraform init` を実行する
- `terraform validate` を実行する
- 実行順は `fmt → init → validate` にする
- 実行対象は `terraform/` ディレクトリとする

> ヒント:
>
> - `terraform validate` は `init` より先に実行すると失敗します
> - このプラクティスでは local backend を使うため、remote backend の初期化は扱いません

必要に応じて、次の公式ドキュメントを参照してください。

- [terraform init](https://developer.hashicorp.com/terraform/cli/commands/init)
- [terraform validate](https://developer.hashicorp.com/terraform/cli/commands/validate)

## 確認

- 変更を push し、`init` と `validate` が成功することを確認する
- `terraform/main.tf` に構文エラー（例: `}` を削除）を入れて push し、`validate` で失敗することを確認する
- 確認後、構文エラーを元に戻して push する

---

次のプラクティス：[4-5. terraform plan で変更内容を確認する](./4-5-plan.md)
