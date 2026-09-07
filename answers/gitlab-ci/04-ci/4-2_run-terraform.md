# 解答例：4-2. CI から Terraform を実行できるようにする

## 解答

### 1. `ci/terraform.gitlab-ci.yml` を新規に作成する

```yaml
terraform:
  image:
    name: "hashicorp/terraform:$TF_VERSION"
    entrypoint: [""]
  id_tokens:
    GITLAB_OIDC_TOKEN:
      aud: sts.amazonaws.com
  before_script:
    - echo "$GITLAB_OIDC_TOKEN" > /tmp/gitlab-oidc-token
    - export AWS_WEB_IDENTITY_TOKEN_FILE=/tmp/gitlab-oidc-token
  script:
    - terraform version
```

### 2. `.gitlab-ci.yml` から include する

```yaml
include:
  - local: ci/terraform.gitlab-ci.yml
```

## 解説

- `image.name` には Terraform イメージを指定し、バージョンは GitLab の CI/CD Variables で設定した `TF_VERSION` で管理しています。
- `entrypoint: [""]` は、`hashicorp/terraform` イメージの entrypoint を無効にして、GitLab が `script` をそのままシェル実行できるようにするための設定です。このイメージは `terraform` コマンドを直接実行する前提で entrypoint が設定されており、これはバージョンによらず共通の仕様なので、常に必要な設定です。
- `id_tokens` で OIDC トークンを受け取り、Audience を `sts.amazonaws.com` に設定しています。
- `before_script` でトークンを `/tmp/gitlab-oidc-token` に保存し、`AWS_WEB_IDENTITY_TOKEN_FILE` を設定することで、後続で AWS の Web Identity 認証に使えるようにしています。
- この段階では `terraform version` を実行して、Terraform 環境と OIDC の準備が正しくできていることを確認します。

---

[目次に戻る](../../README.md)
