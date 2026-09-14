# 解答例：5-5. plan 結果を利用して terraform apply を実行する

## 解答

以下が Step 5 の完成形です。

```yaml
workflow:
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event" && $CI_MERGE_REQUEST_TARGET_BRANCH_NAME == "main"'
    - if: '$CI_PIPELINE_SOURCE == "push" && $CI_COMMIT_BRANCH == "main"'
    - if: '$CI_PIPELINE_SOURCE == "web"'
    - when: never

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

approve_apply:
  stage: deploy
  when: manual
  script:
    - echo "approve apply"

apply:
  stage: deploy
  needs:
    - job: plan
      artifacts: true
    - job: approve_apply
  image:
    name: "hashicorp/terraform:$TF_VERSION"
    entrypoint: [""]
  id_tokens:
    GITLAB_OIDC_TOKEN:
      aud: sts.amazonaws.com
  before_script:
    - echo "$GITLAB_OIDC_TOKEN" > /tmp/gitlab-oidc-token
    - export AWS_WEB_IDENTITY_TOKEN_FILE=/tmp/gitlab-oidc-token
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
      when: never
    - if: '$CI_COMMIT_BRANCH == "main"'
    - when: never
  environment:
    name: production
  script:
    - terraform -chdir=terraform init
    - terraform -chdir=terraform apply tfplan
```

## 解説

- `plan` job で作成した `tfplan` は `artifacts` に保存されます。
- `needs:` の中で `artifacts: true` を指定することで、`plan` job の成果物を `apply` job で取得できます。
- `approve_apply` を `when: manual` にして、承認待ちのような動作を再現しています。
- `needs` の依存関係に `approve_apply` を追加することで、承認後に `apply` が実行されます。
- `terraform -chdir=terraform apply tfplan` で、保存した plan をそのまま適用します。
- `rules` により、PR 時には apply を止め、main ブランチのときだけ apply を実行するようにしています。
- `environment: production` を付けることで、対象環境へのデプロイ意図を明確にしています。

これが Step 5 の完成形です。

---

[目次に戻る](../../README.md)
