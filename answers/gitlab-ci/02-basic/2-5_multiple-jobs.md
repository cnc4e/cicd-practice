# 解答例：2-5. 複数の job を作る

## 解答

`.gitlab-ci.yml` を以下の内容に修正します。

```yaml
job1:
  image: alpine:latest
  script:
    - echo "job1"

job2:
  image: alpine:latest
  script:
    - echo "job2"
```

## 解説

- 2-4 で追加した `rules` を削除し、push で実行される状態に戻しています。
- `.gitlab-ci.yml` のトップレベルに複数の job を並べることで、複数の job を定義できます。
- `stage` を指定しない場合、すべての job はデフォルトの `test` ステージに属し、**並列**に実行されます。
- GitLab の CI/CD > Pipelines 画面から実行結果を開くと、2 つの job が同時に実行されていることを確認できます。

---

[目次に戻る](../../README.md)
