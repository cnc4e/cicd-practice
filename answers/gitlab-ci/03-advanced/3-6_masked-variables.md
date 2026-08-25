# 解答例：3-6. masked variables で機密情報を扱う

## 解答

### 1. プロジェクトに masked variable を作成する

GitLab のプロジェクトページから **Settings > CI/CD > Variables** を開き、以下の variable を作成します。

| Key             | Value                                      | Masked | Protected |
| --------------- | ------------------------------------------ | ------ | --------- |
| `SAMPLE_SECRET` | 任意の文字列（例：`my-secret-value-1234`） | Yes    | No        |

### 2. `ci/3-6-masked-variables.gitlab-ci.yml` を新規に作成する

```yaml
use-secret:
  image: alpine
  script:
    - echo "$SAMPLE_SECRET"
```

### 3. `.gitlab-ci.yml` から include する

```yaml
include:
  - local: ci/3-6-masked-variables.gitlab-ci.yml
```

## 解説

- variable の追加・編集画面で **Mask variable** にチェックを入れると、Masked variable になります。
- Masked variable の値は、実行ログ上で `[MASKED]` と表示され、内容が隠されます。
- Masked に設定するには、値が 8 文字以上であることと、特定の特殊文字（改行など）を含まないことが条件です。
- GitLab の Masked variable は GitHub Actions の secrets に相当します。
- Protected を有効にすると、Protected ブランチや Protected タグでのみ変数が参照できるようになります。

---

[目次に戻る](../../README.md)
