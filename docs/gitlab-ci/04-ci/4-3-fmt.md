# 4-3. terraform fmt でフォーマットを確認する

> **前提**: この課題は [4-2. CI から Terraform を実行できるようにする](./4-2-run-terraform.md) を完了していることを前提とします。

`terraform fmt` は Terraform コードを標準的なフォーマットに整えるコマンドです。
`-check` を付けると、フォーマットが崩れている場合に終了コードが 0 以外になるため、CI で検出できます。

## プラクティス

`ci/terraform.gitlab-ci.yml` を編集し、次の条件を満たしてください。

条件は次のとおりです。

- `terraform fmt -check` を実行するようにする
- 対象は `terraform/` ディレクトリ配下とする

> ヒント:
>
> - `terraform -chdir=terraform fmt -check` のように `-chdir` を使うと、対象ディレクトリを指定できます

必要に応じて、次の公式ドキュメントを参照してください。

- [terraform fmt](https://developer.hashicorp.com/terraform/cli/commands/fmt)

## 確認

- 変更を push し、pipeline が成功することを確認する
- `terraform/main.tf` のインデントを意図的に崩して push し、CI が失敗することを確認する
- 確認後、フォーマットを元に戻して push する

---

次のプラクティス：[4-4. terraform init / validate で構成を検証する](./4-4-validate.md)
