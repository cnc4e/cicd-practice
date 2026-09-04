# 3-4. ブランチによって実行する job を変える

> **前提**: この課題は [3-2. `rules` で job の実行条件を分ける](./3-2-rules.md) を完了していることを前提とします。

GitLab CI/CD では、どのブランチで pipeline が動いているかも参照できます。

3-3 ではイベントの種別を条件にしましたが、ブランチ名も同様に `rules` の `if` で使えます。`$CI_COMMIT_BRANCH` でブランチ名を参照でき、main ブランチのときだけ特定の処理を実行するといった制御ができます。実際の CI/CD でも、ブランチによって処理を分けることはよくあります。

## プラクティス

次の条件を満たす pipeline を `ci/3-4-branch-condition.gitlab-ci.yml` として新規に作成し、`.gitlab-ci.yml` から `include` で取り込んでください。

条件は次のとおりです。

- `rules` を使ってブランチ条件を定義する
- main ブランチのときだけ実行される job を 1 つ定義する
- その job では `echo "run on main"` を実行する

> ヒント:
>
> - `$CI_COMMIT_BRANCH` に現在のブランチ名が格納されています
> - ただし Merge Request pipeline では `$CI_COMMIT_BRANCH` ではなく `$CI_MERGE_REQUEST_TARGET_BRANCH_NAME` を参照することがあります

必要に応じて、次の公式ドキュメントを参照してください。

- [Predefined CI/CD variables reference](https://docs.gitlab.com/ci/variables/predefined_variables/)
- [Control jobs in a pipeline - rules](https://docs.gitlab.com/ci/jobs/job_rules/)

## 確認

- main 以外のブランチで変更を push し、pipeline を開く
- `run on main` の job が実行されないことを確認する
- main ブランチにマージ（または直接 push）し、pipeline を開く
- `run on main` の job が実行されることを確認する

---

次のプラクティス：[3-5. variables を使って値を外部で管理する](./3-5-variables.md)
