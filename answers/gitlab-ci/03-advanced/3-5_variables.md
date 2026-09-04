# 解答例：3-5. variables を使って値を外部で管理する

## 解答

### 1. プロジェクトに variable を作成する

GitLab のプロジェクトページから **Settings > CI/CD > Variables** を開き、以下の variable を作成します。

| Key              | Value                                     | Masked | Protected |
| ---------------- | ----------------------------------------- | ------ | --------- |
| `SAMPLE_MESSAGE` | 任意の文字列（例：`Hello from variable`） | No     | No        |

### 2. `ci/3-5-variables.gitlab-ci.yml` を新規に作成する

```yaml
print-variable:
  image: alpine
  script:
    - echo "$SAMPLE_MESSAGE"
```

### 3. `.gitlab-ci.yml` から include する

```yaml
include:
  - local: ci/3-5-variables.gitlab-ci.yml
```

## 解説

- GitLab CI/CD の variables は、Settings > CI/CD > Variables から管理します。
- pipeline の中では `$変数名` の形式で参照できます。
- Masked を設定しない variable の値はログにそのまま表示されます。
- 環境ごとに値を変えたい場合や、YAML に直接書きたくない設定値を管理するのに向いています。
- GitHub Actions の `vars` context に相当する仕組みです。

---

[目次に戻る](../../README.md)
