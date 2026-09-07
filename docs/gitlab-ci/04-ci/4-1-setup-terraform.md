# 4-1. Terraform コードを準備する

Step 4 では Terraform コードを GitLab CI/CD で検証します。
まず、このリポジトリに用意されている Terraform サンプルコードを、学習用リポジトリにコピーします。

## プラクティス

`cicd-practice/terraform/` ディレクトリを、学習用リポジトリにコピーしてください。

コピー後の学習用リポジトリの構成例は次のとおりです。

```text
(学習用リポジトリ)/
├── .gitlab-ci.yml           # Step 2/3 で作成した設定
├── ci/                      # Step 3 で追加した include 用ファイル
└── terraform/
    ├── main.tf
    ├── variables.tf
    └── .gitignore
```

> **補足: .terraform.lock.hcl について**
>
> `terraform init` を実行すると `.terraform.lock.hcl` が生成されます。
> このファイルは provider のバージョンを固定するためのものなので、Git 管理に含めることが推奨されています。

> **補足: backend について**
>
> このプラクティスでは local backend を使用します。
> Step 4 では CI 上で `fmt` / `validate` / `plan` を実行する流れの理解を優先します。

## 確認

- 学習用リポジトリに `terraform/main.tf` があることを確認する
- `terraform/.gitignore` により `.terraform/` と `*.tfplan` が Git 管理対象外になっていることを確認する
- 変更を push する

---

次のプラクティス：[4-2. CI から Terraform を実行できるようにする](./4-2-run-terraform.md)
