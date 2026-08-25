# 解答例：3-4. ブランチによって実行する job を変える

## 解答

### 1. `ci/3-4-branch-condition.gitlab-ci.yml` を新規に作成する

```yaml
run-on-main:
  image: alpine
  rules:
    - if: $CI_COMMIT_BRANCH == "main"
  script:
    - echo "run on main"
```

### 2. `.gitlab-ci.yml` から include する

```yaml
include:
  - local: ci/3-4-branch-condition.gitlab-ci.yml
```

## 解説

- `$CI_COMMIT_BRANCH` は現在の push が行われたブランチ名を保持する定義済み変数です。
- `rules: - if: $CI_COMMIT_BRANCH == "main"` とすることで、main ブランチへの push のときだけ job が実行されます。
- Merge Request pipeline では `$CI_COMMIT_BRANCH` が設定されないため、この条件では MR pipeline で job は実行されません。Merge Request でもブランチ条件を使いたい場合は `$CI_MERGE_REQUEST_TARGET_BRANCH_NAME` を合わせて確認してください。
- GitHub Actions の `github.ref` に相当する情報が、GitLab では `$CI_COMMIT_BRANCH` や `$CI_COMMIT_REF_NAME` などに分かれています。

---

[目次に戻る](../../README.md)
