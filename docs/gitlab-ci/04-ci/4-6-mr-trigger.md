# 4-6. Merge Request で CI を実行する

> **前提**: この課題は [4-5. terraform plan で変更内容を確認する](./4-5-plan.md) を完了していることを前提とします。

ここまでは push や手動実行で確認してきました。
実際の CI では、Merge Request をきっかけに自動で検証を実行する構成がよく使われます。

この課題では、Step 3 で学んだ `rules` / `workflow:rules` を使って、実行タイミングを制御します。

## プラクティス

`ci/terraform.gitlab-ci.yml` を編集し、次の条件を満たしてください。

条件は次のとおりです。

- Merge Request をきっかけに CI が実行されるようにする
- 対象は main ブランチ向けの Merge Request のみとする
- 手動実行（CI/CD > Pipelines の Run pipeline）でも CI を実行できるようにする
- 条件に一致しない場合は実行しないようにする

> ヒント:
>
> - Merge Request 起点は `$CI_PIPELINE_SOURCE == "merge_request_event"` で判定できます
> - ターゲットブランチは `$CI_MERGE_REQUEST_TARGET_BRANCH_NAME` で参照できます
> - 手動実行は `$CI_PIPELINE_SOURCE == "web"` で判定できます

必要に応じて、次の公式ドキュメントを参照してください。

- [Control jobs in a pipeline - rules](https://docs.gitlab.com/ci/jobs/job_rules/)
- [CI/CD YAML syntax reference - workflow:rules](https://docs.gitlab.com/ci/yaml/#workflowrules)
- [Predefined CI/CD variables reference](https://docs.gitlab.com/ci/variables/predefined_variables.html)

## 確認

- 変更を push し、main 以外のブランチから main への Merge Request を作成する
- Merge Request 作成時に CI が自動実行されることを確認する
- CI/CD > Pipelines から手動実行し、CI が実行されることを確認する

---

次のプラクティス：[4-7. plan 結果を artifact として保存する](./4-7-artifact.md)
