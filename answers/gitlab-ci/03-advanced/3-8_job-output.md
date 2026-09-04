# 解答例：3-8. job 間で値を受け渡す

## 解答

### 1. `ci/3-8-job-output.gitlab-ci.yml` を新規に作成する

```yaml
job1:
  image: alpine
  script:
    - echo "MESSAGE=Hello from job1" > output.env
  artifacts:
    reports:
      dotenv: output.env

job2:
  image: alpine
  needs:
    - job: job1
      artifacts: true
  script:
    - echo "$MESSAGE"
```

### 2. `.gitlab-ci.yml` から include する

```yaml
include:
  - local: ci/3-8-job-output.gitlab-ci.yml
```

## 解説

- `echo "MESSAGE=Hello from job1" > output.env` で、`KEY=VALUE` 形式の dotenv ファイルを作成しています。
- `artifacts:reports:dotenv` に指定すると、ファイルの内容が後続 job の環境変数として自動的に展開されます。
- `needs` に `artifacts: true` を指定すると、`job1` の artifacts を `job2` で受け取れます。
- `job2` では `$MESSAGE` として参照するだけで、`Hello from job1` が出力されます。
- GitHub Actions の `$GITHUB_OUTPUT` と `needs.<job_id>.outputs` に相当する仕組みです。

---

[目次に戻る](../../README.md)
