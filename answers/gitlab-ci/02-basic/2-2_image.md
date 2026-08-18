# 解答例：2-2. `image` で実行環境を指定する

## 解答

`.gitlab-ci.yml` を以下の内容に修正します。

```yaml
hello:
  image: alpine:latest
  script:
    - echo "Hello, GitLab CI/CD"
```

## 解説

- `image: alpine:latest` を job に追加することで、Alpine Linux の最新環境で job が実行されます。
- `image` は job ごとに指定できます。pipeline 全体に共通の image を設定することもできます。
- push 後に GitLab の CI/CD > Pipelines 画面を開くと、pipeline の実行結果を確認できます。

---

[目次に戻る](../../README.md)
