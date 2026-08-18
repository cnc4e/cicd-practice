# 解答例：2-6. `stages` で job の実行順を制御する

## 解答

`.gitlab-ci.yml` を以下の内容に修正します。

```yaml
stages:
  - first
  - second

job1:
  stage: first
  image: alpine:latest
  script:
    - echo "first job"

job2:
  stage: second
  image: alpine:latest
  script:
    - echo "second job"
```

## 解説

- `stages` にステージ名を並べた順番が、実行順になります。
- 各 job の `stage` に、対応するステージ名を指定します。
- `job1` が `first` ステージ、`job2` が `second` ステージに属するため、`job1` が完了してから `job2` が実行されます。
- `job1` が失敗した場合、`job2` は実行されません。
- GitLab の CI/CD > Pipelines 画面から実行結果を開くと、`first` → `second` の順でステージが進んでいることを確認できます。

---

[目次に戻る](../../README.md)
