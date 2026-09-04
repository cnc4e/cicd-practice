# 解答例 目次

## GitHub Actions 編

### Step 2: 基礎編

| プラクティス                                       | 解答                                                                                 |
| -------------------------------------------------- | ------------------------------------------------------------------------------------ |
| 2-1. workflow を作成する                           | [2-1_create-workflow.md](./github-actions/02-basic/2-1_create-workflow.md)           |
| 2-2. `runs-on` で実行環境を指定する                | [2-2_runs-on.md](./github-actions/02-basic/2-2_runs-on.md)                           |
| 2-3. 複数の step を定義する                        | [2-3_multiple-steps.md](./github-actions/02-basic/2-3_multiple-steps.md)             |
| 2-4. Pull Request をきっかけに workflow を実行する | [2-4_pull-request-trigger.md](./github-actions/02-basic/2-4_pull-request-trigger.md) |
| 2-5. 複数の job を作る                             | [2-5_multiple-jobs.md](./github-actions/02-basic/2-5_multiple-jobs.md)               |
| 2-6. `needs` で job の実行順を制御する             | [2-6_needs.md](./github-actions/02-basic/2-6_needs.md)                               |

### Step 3: 応用編

| プラクティス                                | 解答                                                                            |
| ------------------------------------------- | ------------------------------------------------------------------------------- |
| 3-1. `if` で step の実行条件を分ける        | [3-1_if.md](./github-actions/03-advanced/3-1_if.md)                             |
| 3-2. イベントによって実行する step を変える | [3-2_event-condition.md](./github-actions/03-advanced/3-2_event-condition.md)   |
| 3-3. ブランチによって実行する step を変える | [3-3_branch-condition.md](./github-actions/03-advanced/3-3_branch-condition.md) |
| 3-4. variables を使って値を外部で管理する   | [3-4_variables.md](./github-actions/03-advanced/3-4_variables.md)               |
| 3-5. secrets を使って機密情報を扱う         | [3-5_secrets.md](./github-actions/03-advanced/3-5_secrets.md)                   |
| 3-6. job 間で値を受け渡す                   | [3-6_job-output.md](./github-actions/03-advanced/3-6_job-output.md)             |
| 3-7. artifact を保存する                    | [3-7_artifact.md](./github-actions/03-advanced/3-7_artifact.md)                 |

### Step 4: 実践編（CI）

| プラクティス                                    | 解答                                                                |
| ----------------------------------------------- | ------------------------------------------------------------------- |
| 4-2. CI から Terraform を実行できるようにする   | [4-2_run-terraform.md](./github-actions/04-ci/4-2_run-terraform.md) |
| 4-3. terraform fmt でフォーマットを確認する     | [4-3_fmt.md](./github-actions/04-ci/4-3_fmt.md)                     |
| 4-4. terraform init / validate で構成を検証する | [4-4_validate.md](./github-actions/04-ci/4-4_validate.md)           |
| 4-5. terraform plan で変更内容を確認する        | [4-5_plan.md](./github-actions/04-ci/4-5_plan.md)                   |
| 4-6. Pull Request で CI を実行する              | [4-6_pr-trigger.md](./github-actions/04-ci/4-6_pr-trigger.md)       |
| 4-7. plan 結果を artifact として保存する        | [4-7_artifact.md](./github-actions/04-ci/4-7_artifact.md)           |

### Step 5: 実践編（CD）

| プラクティス                                                | 解答                                                            |
| ----------------------------------------------------------- | --------------------------------------------------------------- |
| 5-1. CD workflow のトリガーを設定する                       | [5-1_trigger.md](./github-actions/05-cd/5-1_trigger.md)         |
| 5-2. plan と apply を job 分割する                          | [5-2_job-split.md](./github-actions/05-cd/5-2_job-split.md)     |
| 5-3. apply の実行条件を制御する                             | [5-3_skip-on-pr.md](./github-actions/05-cd/5-3_skip-on-pr.md)   |
| 5-4. GitHub Environments で apply 前の承認を設定する        | [5-4_environment.md](./github-actions/05-cd/5-4_environment.md) |
| 5-5. plan 結果をダウンロードして terraform apply を実行する | [5-5_apply.md](./github-actions/05-cd/5-5_apply.md)             |

### Step 6: Runner 編

| プラクティス                                   | 解答                                                                        |
| ---------------------------------------------- | --------------------------------------------------------------------------- |
| 6-2. GitHub-hosted runner で環境を確認する     | [6-2_github-hosted.md](./github-actions/06-runner/6-2_github-hosted.md)     |
| 6-5. Self-hosted runner で workflow を実行する | [6-5_self-hosted-run.md](./github-actions/06-runner/6-5_self-hosted-run.md) |

---

## GitLab CI/CD 編

### Step 2: 基礎編

| プラクティス                                        | 解答                                                                              |
| --------------------------------------------------- | --------------------------------------------------------------------------------- |
| 2-1. pipeline を作成する                            | [2-1_create-pipeline.md](./gitlab-ci/02-basic/2-1_create-pipeline.md)             |
| 2-2. `image` で実行環境を指定する                   | [2-2_image.md](./gitlab-ci/02-basic/2-2_image.md)                                 |
| 2-3. 複数の script を定義する                       | [2-3_multiple-scripts.md](./gitlab-ci/02-basic/2-3_multiple-scripts.md)           |
| 2-4. Merge Request をきっかけに pipeline を実行する | [2-4_merge-request-trigger.md](./gitlab-ci/02-basic/2-4_merge-request-trigger.md) |
| 2-5. 複数の job を作る                              | [2-5_multiple-jobs.md](./gitlab-ci/02-basic/2-5_multiple-jobs.md)                 |
| 2-6. `stages` で job の実行順を制御する             | [2-6_stages.md](./gitlab-ci/02-basic/2-6_stages.md)                               |

### Step 3: 応用編

| プラクティス                                               | 解答                                                                       |
| ---------------------------------------------------------- | -------------------------------------------------------------------------- |
| 3-1. `include` で pipeline を分割して管理する              | [3-1_include.md](./gitlab-ci/03-advanced/3-1_include.md)                   |
| 3-2. `rules` で job の実行条件を分ける                     | [3-2_rules.md](./gitlab-ci/03-advanced/3-2_rules.md)                       |
| 3-3. イベントによって実行する job を変える                 | [3-3_event-condition.md](./gitlab-ci/03-advanced/3-3_event-condition.md)   |
| 3-4. ブランチによって実行する job を変える                 | [3-4_branch-condition.md](./gitlab-ci/03-advanced/3-4_branch-condition.md) |
| 3-5. variables を使って値を外部で管理する                  | [3-5_variables.md](./gitlab-ci/03-advanced/3-5_variables.md)               |
| 3-6. masked variables で機密情報を扱う                     | [3-6_masked-variables.md](./gitlab-ci/03-advanced/3-6_masked-variables.md) |
| 3-7. artifact を保存する                                   | [3-7_artifact.md](./gitlab-ci/03-advanced/3-7_artifact.md)                 |
| 3-8. job 間で値を受け渡す                                  | [3-8_job-output.md](./gitlab-ci/03-advanced/3-8_job-output.md)             |
| 3-9. `workflow:rules` で pipeline 全体の実行条件を制御する | [3-9_workflow-rules.md](./gitlab-ci/03-advanced/3-9_workflow-rules.md)     |
