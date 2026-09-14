# 解答例：4-3. terraform fmt でフォーマットを確認する

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
```

## 解説

- `terraform -chdir=terraform fmt -check` により、`terraform/` ディレクトリ配下のフォーマットを検証しています。
- `-check` を付けることで、フォーマット崩れがあると job が失敗し、CI で検出できます。

---

[目次に戻る](../../README.md)
