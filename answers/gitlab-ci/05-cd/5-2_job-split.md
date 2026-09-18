# 解答例：5-2. plan と apply を job 分割する

## 解答

`ci/terraform.gitlab-ci.yml` の job は、次のように分割します。

```yaml
stages:
  - test
  - deploy

plan:
  stage: test
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
  artifacts:
    paths:
      - terraform/tfplan
    expire_in: 1 week

apply:
  stage: deploy
  needs: ["plan"]
  image:
    name: "hashicorp/terraform:$TF_VERSION"
    entrypoint: [""]
  script:
    - echo "apply"
```

## 解説

- `plan` job は Step 4 の内容をそのまま維持しつつ、`stage: test` を明示しています。
- `apply` job は `needs: ["plan"]` により、`plan` job が成功した後にのみ実行されます。
- `stages` を `test` と `deploy` に分けることで、`plan` は検証フェーズ、`apply` はデプロイフェーズとして役割を分離できます。

この状態で、plan が成功した場合にのみ apply の実行順が決まります。

---

[目次に戻る](../../README.md)
