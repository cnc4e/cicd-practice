# 解答例：5-3. apply の実行条件を制御する

## 解答

`apply` job に次の `rules` を追加します。

```yaml
apply:
  stage: deploy
  needs: ["plan"]
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

- `merge_request_event` のときは `when: never` で apply をスキップします。
- `CI_COMMIT_BRANCH == "main"` のときだけ `apply` が実行されます。
- main 以外のブランチからの手動実行も `when: never` により止められます。

これで、PR では plan のみを確認し、main への push や main からの手動実行だけで apply を実行する構成になります。

---

[目次に戻る](../../README.md)
