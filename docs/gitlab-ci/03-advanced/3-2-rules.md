# 3-2. `rules` で job の実行条件を分ける

> **前提**: この課題は [3-1. `include` で pipeline を分割して管理する](./3-1-include.md) を完了していることを前提とします。

GitLab CI/CD では、`rules` を使って job の実行条件を定義できます。

`rules` には条件（`if`）と実行の契機（`when`）を組み合わせて書き、条件に応じて job を自動実行するか、手動実行にするか、スキップするかを制御できます。

この課題では、`rules` と `needs` を組み合わせて、1 つ目の job が完了した後に、2 つ目の job を手動でのみ実行できるようにする pipeline を作成します。

## プラクティス

次の条件を満たす pipeline を `ci/3-2-rules.gitlab-ci.yml` として新規に作成し、`.gitlab-ci.yml` から `include` で取り込んでください。

条件は次のとおりです。

- job を 2 つ定義する（`job1`、`job2`）
- `job1` では `echo "job1"` を実行する
- `needs` を使って `job1` → `job2` の順に実行されるようにする
- `job2` に `rules` を定義する
- `job2` は手動でのみ実行できるようにする（自動では実行しない）
- `job2` が手動実行されたとき、`echo "manual job"` を実行する

> ヒント:
>
> - `rules` に `when: manual` を指定すると、job は自動では実行されず、手動でのみ起動できます
> - `allow_failure: true` は、手動 job を pipeline 全体のブロッカーにしないための設定です

必要に応じて、次の公式ドキュメントを参照してください。

- [Control jobs in a pipeline - rules](https://docs.gitlab.com/ci/jobs/job_rules/)
- [CI/CD YAML syntax reference - rules:when](https://docs.gitlab.com/ci/yaml/#ruleswhen)
- [CI/CD YAML syntax reference - needs](https://docs.gitlab.com/ci/yaml/#needs)

## 確認

- 変更を push し、GitLab の CI/CD > Pipelines 画面で pipeline を開く
- `job1` が自動で実行されることを確認する
- `job2` が自動では実行されず、手動で起動できる状態になっていることを確認する
- `job2` を手動で起動し、`manual job` が出力されることを確認する

---

次のプラクティス：[3-3. イベントによって実行する job を変える](./3-3-event-condition.md)
