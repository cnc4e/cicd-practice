# 解答例：2-1. pipeline を作成する

## 解答

リポジトリのルートに `.gitlab-ci.yml` を以下の内容で作成します。

```yaml
hello:
  script:
    - echo "Hello, GitLab CI/CD"
```

## 解説

- job 名（`hello`）をトップレベルに記述します。
- `script` に実行するコマンドを列挙します。
- この段階では `image` を指定していないため、runner の設定によっては job が失敗することがあります。
- pipeline 自体が CI/CD > Pipelines 画面に表示されていれば、.gitlab-ci.yml が正しく認識されています。

---

[目次に戻る](../../README.md)
