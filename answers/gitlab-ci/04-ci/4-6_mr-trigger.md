# 解答例：4-6. Merge Request で CI を実行する

## 解答

`ci/terraform.gitlab-ci.yml` を以下のように編集します。

```yaml
workflow:
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event" && $CI_MERGE_REQUEST_TARGET_BRANCH_NAME == "main"'
    - if: '$CI_PIPELINE_SOURCE == "web"'
    - when: never

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

- `workflow: rules` によって pipeline 全体の起動条件を制御しています。
- 1つ目の条件で「main 向け Merge Request」のときだけ自動実行します。
- 2つ目の条件で `Run pipeline`（`web`）による手動実行も許可しています。
- 最後の `when: never` で、それ以外のイベントでは実行しないようにしています。

---

[目次に戻る](../../README.md)
