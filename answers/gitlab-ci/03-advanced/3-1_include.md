# 解答例：3-1. `include` で pipeline を分割して管理する

## 解答

### 1. `ci/hello.gitlab-ci.yml` を新規に作成する

```yaml
hello:
  image: alpine
  script:
    - echo "Hello from include"
```

### 2. `.gitlab-ci.yml` に include を追加する

```yaml
include:
  - local: ci/hello.gitlab-ci.yml
```

## 解説

- `include: - local:` を使うと、同じリポジトリ内の別ファイルを `.gitlab-ci.yml` に取り込めます。
- 取り込んだファイルの job は、`.gitlab-ci.yml` に直接書いたものと同じように動作します。
- Step 3 以降の課題では、確認したい内容を `ci/` 以下のファイルに分けて定義し、この `include` の仕組みで取り込みます。
- 複数ファイルを取り込む場合は、リストに並べます。

```yaml
include:
  - local: ci/3-2-rules.gitlab-ci.yml
  - local: ci/3-3-event-condition.gitlab-ci.yml
```

---

[目次に戻る](../../README.md)
