# 解答例：4-5. terraform plan で変更内容を確認する

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
    - terraform -chdir=terraform plan -var="bucket_name=$BUCKET_NAME" -out=tfplan
```

## 解説

- `terraform plan` は実際の変更を適用せず、変更内容だけを確認できます。
- `-var="bucket_name=$BUCKET_NAME"` で CI/CD Variables から値を受け取り、環境依存の値を YAML の外で管理できます。
- `-out=tfplan` により plan 結果をファイルとして保存します。次の課題で artifact として保存します。

---

[目次に戻る](../../README.md)
