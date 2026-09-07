# 4-2. CI から Terraform を実行できるようにする

CI から Terraform コマンドを実行するには、Terraform が使える実行環境を job に用意する必要があります。
また、AWS に接続するために GitLab の OIDC トークンを受け取り、Web Identity 認証の準備を行います。

この課題では、Terraform を実行できる image と OIDC の設定をまとめて定義し、まずは `terraform version` が実行できることを確認します。

## プラクティス

次の条件を満たす pipeline を `ci/terraform.gitlab-ci.yml` として新規に作成し、`.gitlab-ci.yml` から `include` で取り込んでください。

条件は次のとおりです。

- `terraform` という名前の job を 1 つ作成する
- `image` は Terraform イメージを指定する（バージョンは変数で管理する）
- `image` には `entrypoint: [""]` を設定し、コンテナのデフォルト entrypoint を無効化する
- `id_tokens` で `GITLAB_OIDC_TOKEN` を定義し、Audience に `sts.amazonaws.com` を指定する
- `before_script` で OIDC トークンをファイルに保存し、`AWS_WEB_IDENTITY_TOKEN_FILE` を設定する
- `script` で `terraform version` を実行する
- GitLab の CI/CD Variables に Terraform のバージョンを指定する変数（例: `TF_VERSION`）を追加する

> ヒント:
>
> - GitLab では `id_tokens` を使って OIDC トークンを取得できます
> - `AWS_WEB_IDENTITY_TOKEN_FILE` を設定すると、AWS の Web Identity 認証に使えます
> - `hashicorp/terraform` イメージは `terraform` コマンドを直接実行する前提で entrypoint が設定されているため、GitLab の shell 実行と竞合します。`entrypoint: [""]` で無効化してください

必要に応じて、次の公式ドキュメントを参照してください。

- [GitLab CI/CD with OpenTofu and Terraform](https://docs.gitlab.com/user/infrastructure/iac/)
- [ID token authentication](https://docs.gitlab.com/ci/secrets/id_token_authentication/)
- [terraform version](https://developer.hashicorp.com/terraform/cli/commands/version)

## 確認

- 変更を push し、pipeline を実行する
- `terraform version` が実行され、指定した Terraform のバージョンが表示されることを確認する

---

次のプラクティス：[4-3. terraform fmt でフォーマットを確認する](./4-3-fmt.md)
