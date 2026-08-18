# 解答例：2-4. Merge Request をきっかけに pipeline を実行する

## 解答

`.gitlab-ci.yml` を以下の内容に修正します。

```yaml
hello:
  image: alpine:latest
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
  script:
    - echo "step1"
    - echo "step2"
```

## 解説

- `rules` を使って pipeline の実行条件を制御しています。
- `$CI_PIPELINE_SOURCE == "merge_request_event"` は、Merge Request のときだけ job を実行する条件です。
- この条件を指定すると、push だけでは pipeline が起動しなくなります。
- Merge Request を作成または更新したタイミングで pipeline が実行されます。
- CI/CD > Pipelines 画面でソースの列が `merge_request_event` と表示されていれば正解です。

---

[目次に戻る](../../README.md)
