# 3-3. イベントによって実行する job を変える

> **前提**: この課題は [3-2. `rules` で job の実行条件を分ける](./3-2-rules.md) を完了していることを前提とします。

GitLab CI/CD では、pipeline を起動したイベントの情報を参照できます。

3-2 では `rules` の `when` で実行タイミングを制御しましたが、`rules` の `if` にはイベントの種別も使えます。Merge Request によって起動したのか、push によって起動したのかを `$CI_PIPELINE_SOURCE` 変数で判別し、実行する job を分けることができます。

## プラクティス

次の条件を満たす pipeline を `ci/3-3-event-condition.gitlab-ci.yml` として新規に作成し、`.gitlab-ci.yml` から `include` で取り込んでください。

条件は次のとおりです。

- `rules` を使って実行条件を制御する
- Merge Request のときだけ実行される job を 1 つ定義する
- その job では `echo "run on merge request"` を実行する
- Merge Request 以外のとき（push など）だけ実行される job を 1 つ定義する
- その job では `echo "run on push"` を実行する

> ヒント:
>
> - `$CI_PIPELINE_SOURCE` に pipeline の起動元が格納されています
> - Merge Request による起動は `"merge_request_event"` になります

必要に応じて、次の公式ドキュメントを参照してください。

- [Predefined CI/CD variables reference](https://docs.gitlab.com/ci/variables/predefined_variables/)
- [Control jobs in a pipeline - rules](https://docs.gitlab.com/ci/jobs/job_rules/)

## 確認

- 変更を push し、GitLab の CI/CD > Pipelines 画面で pipeline を開く
- push 時は `run on push` の job だけが実行されることを確認する
- Merge Request を作成して pipeline を起動する
- `run on merge request` の job だけが実行されることを確認する

---

次のプラクティス：[3-4. ブランチによって実行する job を変える](./3-4-branch-condition.md)
