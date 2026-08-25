# 解答例：3-2. `rules` で job の実行条件を分ける

## 解答

### 1. `ci/3-2-rules.gitlab-ci.yml` を新規に作成する

```yaml
job1:
  image: alpine
  script:
    - echo "job1"

job2:
  image: alpine
  needs:
    - job1
  rules:
    - when: manual
      allow_failure: true
  script:
    - echo "manual job"
```

### 2. `.gitlab-ci.yml` から include する

```yaml
include:
  - local: ci/3-2-rules.gitlab-ci.yml
```

## 解説

- `rules: - when: manual` を指定すると、job は自動では実行されず、pipeline 画面から手動で起動できる状態になります。
- `allow_failure: true` は、手動 job が実行されていない状態でも pipeline 全体が「blocked」にならないようにするための設定です。指定しない場合、未実行の手動 job が pipeline をブロックします。
- `needs: [job1]` により、`job2` は `job1` の完了後に実行できる状態になります。
- GitHub Actions の `if` が step 単位で制御するのに対し、GitLab CI/CD の `rules` は job 単位で制御します。

---

[目次に戻る](../../README.md)
