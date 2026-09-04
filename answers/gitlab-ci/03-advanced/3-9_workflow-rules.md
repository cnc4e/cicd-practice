# 解答例：3-9. `workflow:rules` で pipeline 全体の実行条件を制御する

## 解答

### 1. `ci/3-9-workflow-rules.gitlab-ci.yml` を新規に作成する

```yaml
workflow:
  rules:
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"
    - if: $CI_COMMIT_BRANCH == "main"
    - when: never

check:
  image: alpine
  script:
    - echo "pipeline started"
```

### 2. `.gitlab-ci.yml` から include する

```yaml
include:
  - local: ci/3-9-workflow-rules.gitlab-ci.yml
```

## 解説

- `workflow:rules` はファイルのトップレベルに記述し、pipeline 全体の起動可否を制御します。
- `rules` のリストは上から順に評価され、最初に一致した条件が適用されます。
- `if: $CI_PIPELINE_SOURCE == "merge_request_event"` — Merge Request による起動のとき pipeline を作成します（`when` を省略すると `on_success` 扱いになり、pipeline は起動します）。
- `if: $CI_COMMIT_BRANCH == "main"` — main ブランチへの push のとき pipeline を作成します。
- `when: never` — 上記のどの条件にも合致しない場合は pipeline を作成しません。これがないと、条件に合致しないケースでもデフォルトで pipeline が起動します。
- job の `rules` と違い、`workflow:rules` で除外された場合は pipeline 自体が作成されないため、CI/CD > Pipelines 画面に履歴が残りません。
- 実務では `workflow:rules` を `.gitlab-ci.yml` に直接書くことが一般的です。この課題では Step 3 の構成に合わせて include ファイルに定義していますが、`workflow:rules` は `.gitlab-ci.yml` に直接書いても同様に動作します。

---

[目次に戻る](../../README.md)
