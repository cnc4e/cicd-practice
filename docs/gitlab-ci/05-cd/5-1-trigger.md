# 5-1. CD pipeline のトリガーを設定する

> **前提**: この課題は [Step 4: 実践編（CI）](../04-ci/README.md) を完了していることを前提とします。

Step 4 では、Merge Request をきっかけに CI を実行する pipeline を構築しました。

CD では、main ブランチの更新が取り込まれたタイミングで apply を実行できるようにする必要があります。そこで、**main ブランチへの push** をトリガーに追加し、手動実行も残す構成にします。

この課題では、`ci/terraform.gitlab-ci.yml` の `workflow: rules` を更新して、CD 用の実行条件を追加します。

> 補足:
>
> apply を実行するタイミングは、プロジェクトや運用方針によって異なります。たとえば、main へマージした後に自動で apply する構成や、main への push 直後は plan のみ実行して、deploy は手動承認後に実行する構成などがあります。
>
> このプラクティスでは、main ブランチへの push と手動実行を利用する、シンプルな CD 構成を想定しています。

## プラクティス

`ci/terraform.gitlab-ci.yml` の `workflow: rules` を、次の条件を満たすように更新してください。

条件は次のとおりです。

- 既存の `merge_request_event` 条件はそのまま残す
- `push` による実行を追加する
  - `main` ブランチへの push のときだけ実行する
- 手動実行（`web`）を残す
- 上記以外の実行は無効にする

> ヒント:
>
> - `workflow: rules` には複数の条件を並べて定義できます
> - `main` ブランチ条件は `CI_COMMIT_BRANCH` や `CI_COMMIT_REF_NAME` で確認できます
> - Step 3（3-2、3-4）で学んだイベント条件とブランチ条件を組み合わせます

> 必要に応じて、次の公式ドキュメントを参照してください。
>
> - [GitLab CI/CD Pipeline Configuration Reference - workflow](https://docs.gitlab.com/ci/yaml/workflow/)
> - [GitLab CI/CD Variables](https://docs.gitlab.com/ci/variables/)

## 確認

- feature ブランチで変更を push し、main への Merge Request を作成する
  - MR によって pipeline が実行されることを確認する
- main ブランチに push する
  - `push` によって pipeline が実行されることを確認する
- GitLab の UI から手動実行ができることを確認する

---

次のプラクティス：[5-2. plan と apply を job 分割する](./5-2-job-split.md)
