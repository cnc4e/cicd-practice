# 解答例：5-4. apply 前に手動承認を挟む

## 解答

GitLab の Deployment approvals は、現在の公式ドキュメントでは **Premium / Ultimate** で利用可能です。そのためこの教材では、無料プランでも体験できるように `when: manual` を使った手動承認 job を追加します。

```yaml
approve_apply:
  stage: deploy
  when: manual
  script:
    - echo "approve apply"

apply:
  stage: deploy
  needs: ["plan", "approve_apply"]
  image:
    name: "hashicorp/terraform:$TF_VERSION"
    entrypoint: [""]
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
      when: never
    - if: '$CI_COMMIT_BRANCH == "main"'
    - when: never
  script:
    - echo "apply"
```

## 解説

- GitLab の Deployment approvals は Premium / Ultimate で提供されるため、無料プランでは代替手段が必要です。
- `when: manual` を使うと、承認待ちのような動作を実現できます。
- `needs: ["plan", "approve_apply"]` により、plan が成功し、承認 job を手動実行したあとで apply が動きます。

Environment を使ったデプロイ管理については、この課題では扱いません。必要に応じて公式ドキュメントの Environments を参照してください。

この構成で「plan は自動、apply は手動承認後に実行」という安全なデプロイ手順を体験できます。

---

[目次に戻る](../../README.md)
