# 3-8. job 間で値を受け渡す

GitLab CI/CD では、`artifacts` の dotenv 形式を使って、ある job で作成した値を後続 job に引き渡せます。

dotenv 形式とは `KEY=VALUE` の形で値を記述するテキスト形式です。この形式で書いたファイルを `artifacts:reports:dotenv` で保存すると、後続 job でそのキーを環境変数として参照できます。

たとえば、次のような値を後続 job で使いたい場合に利用できます。

- バージョン番号
- 環境名
- 生成した短い文字列

## プラクティス

次の条件を満たす pipeline を `ci/3-8-job-output.gitlab-ci.yml` として新規に作成し、`.gitlab-ci.yml` から `include` で取り込んでください。

条件は次のとおりです。

- job を 2 つ定義する（`job1`、`job2`）
- `job1` で dotenv 形式のファイルを作成し、artifact として保存する
  - ファイル名は `output.env` にする
  - 書き込む内容は `MESSAGE=Hello from job1` にする
  - `artifacts:reports:dotenv` で保存する
- `needs` を使って `job1` → `job2` の順に実行されるようにする
- `job2` で `job1` の artifact を受け取り、`$MESSAGE` を `echo` で出力する

> ヒント:
>
> - `needs` に `artifacts: true` を指定すると、前の job の artifacts を受け取れます
> - dotenv で保存した値は、後続 job で環境変数として自動的に展開されます

必要に応じて、次の公式ドキュメントを参照してください。

- [CI/CD YAML syntax reference - artifacts:reports:dotenv](https://docs.gitlab.com/ci/yaml/artifacts_reports/#artifactsreportsdotenv)
- [Pass an environment variable to another job](https://docs.gitlab.com/ci/variables/#pass-an-environment-variable-to-another-job)

## 確認

- 変更を push し、pipeline を開く
- `job1` が完了した後に `job2` が実行されることを確認する
- `job2` のログに `Hello from job1` が出力されていることを確認する

---

次のプラクティス：[3-9. `workflow:rules` で pipeline 全体の実行条件を制御する](./3-9-workflow-rules.md)
