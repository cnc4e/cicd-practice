# 解答例：4-4. terraform init / validate で構成を検証する

## 解答

`ci/terraform.gitlab-ci.yml` を以下のように編集します。

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
    - terraform -chdir=terraform fmt -check
    - terraform -chdir=terraform init
    - terraform -chdir=terraform validate
```

## 解説

- `terraform init` は provider のダウンロードと初期化を行います。`validate` の前に必要です。
- `terraform validate` は構文や設定の整合性を確認します。
- 実行順を `fmt → init → validate` にすることで、早い段階でスタイルや構文の問題を検出できます。

---

[目次に戻る](../../README.md)
