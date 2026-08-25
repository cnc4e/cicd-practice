# 解答例：3-3. イベントによって実行する job を変える

## 解答

### 1. `ci/3-3-event-condition.gitlab-ci.yml` を新規に作成する

```yaml
run-on-merge-request:
  image: alpine
  rules:
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"
  script:
    - echo "run on merge request"

run-on-push:
  image: alpine
  rules:
    - if: $CI_PIPELINE_SOURCE != "merge_request_event"
  script:
    - echo "run on push"
```

### 2. `.gitlab-ci.yml` から include する

```yaml
include:
  - local: ci/3-3-event-condition.gitlab-ci.yml
```

## 解説

- `$CI_PIPELINE_SOURCE` は GitLab が自動的に設定する定義済み変数で、pipeline の起動元を表します。
- Merge Request による起動は `"merge_request_event"`、push などによる起動は `"push"` や `"web"` などになります。
- `rules: - if:` に条件式を書くことで、条件に合致した場合のみ job が実行されます。
- 条件に合致しない job は pipeline に表示されません（スキップではなく対象外になります）。

---

[目次に戻る](../../README.md)
