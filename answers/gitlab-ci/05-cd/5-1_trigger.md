# 解答例：5-1. CD pipeline のトリガーを設定する

## 解答

`.gitlab-ci.yml` の `workflow: rules` は、次のように書きます。

```yaml
workflow:
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event" && $CI_MERGE_REQUEST_TARGET_BRANCH_NAME == "main"'
    - if: '$CI_PIPELINE_SOURCE == "push" && $CI_COMMIT_BRANCH == "main"'
    - if: '$CI_PIPELINE_SOURCE == "web"'
    - when: never
```

## 解説

- `merge_request_event` は MR のときに実行されます。
- `push` と `CI_COMMIT_BRANCH == "main"` で main ブランチへの push のときだけ pipeline を起動します。
- `web` は GitLab の UI から手動実行したときに使う実行元です。
- `when: never` を末尾に置くことで、それ以外の pipeline を無効にできます。

これで MR 時と main ブランチへの push のときに pipeline が動き、手動実行も可能になります。

---

[目次に戻る](../../README.md)
