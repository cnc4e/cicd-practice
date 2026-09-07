# 解答例：4-7. plan 結果を artifact として保存する

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
  artifacts:
    paths:
      - terraform/tfplan
    expire_in: 1 week
```

## 解説

- `terraform -chdir=terraform plan -out=tfplan` で作成した plan ファイルを、`artifacts:paths` で保存しています。
- `expire_in` を設定すると、artifact の保持期間を制御できます。不要な蓄積を防ぎ、運用負荷を下げやすくなります。
- `tfplan` は plan を実行した job の実行環境にあるため、plan と artifact 保存は同じ job 内で続けて実行します。

この pipeline が Step 4 の完成形です。Step 5 では保存した plan を利用して apply を実行します。

---

[目次に戻る](../../README.md)
