# 解答例：3-7. artifact を保存する

## 解答

### 1. `ci/3-7-artifact.gitlab-ci.yml` を新規に作成する

```yaml
save-artifact:
  image: alpine
  script:
    - echo "pipeline completed" > result.txt
  artifacts:
    paths:
      - result.txt
    expire_in: 1 week
```

### 2. `.gitlab-ci.yml` から include する

```yaml
include:
  - local: ci/3-7-artifact.gitlab-ci.yml
```

## 解説

- `echo "pipeline completed" > result.txt` で `result.txt` を作成し、文字列を書き込んでいます。
- `artifacts:paths` に保存するファイルのパスを列挙します。
- `artifacts:expire_in` で artifact の有効期限を指定します。指定しない場合はプロジェクト設定の値が使われます。
- 保持期間を明示すると、不要な artifact の蓄積を防ぎ、ストレージ使用量と運用コストを抑えやすくなります。
- 古い artifact が長期間残り続けると、誤って過去の成果物を参照するリスクがあるため、利用目的に合わせた期限設定が有効です。
- 保存した artifact は、GitLab の CI/CD > Pipelines または CI/CD > Jobs 画面の job 実行結果からダウンロードできます。
- GitHub Actions の `actions/upload-artifact` に相当する仕組みです。

---

[目次に戻る](../../README.md)
