# 解答例：2-3. 複数のコマンドを定義する

## 解答

`.gitlab-ci.yml` を以下の内容に修正します。

```yaml
hello:
  image: alpine:latest
  script:
    - echo "step1"
    - echo "step2"
```

## 解説

- `script` はリスト形式で複数のコマンドを定義できます。
- コマンドは上から順番に実行されます。
- `echo "Hello, GitLab CI/CD"` を削除し、2 つのコマンドに置き換えています。
- job のログを開くと、step1 → step2 の順で出力されていることを確認できます。

---

[目次に戻る](../../README.md)
