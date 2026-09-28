# 5-3. apply の実行条件を制御する

> **前提**: この課題は [5-2. plan と apply を job 分割する](./5-2-job-split.md) を完了していることを前提とします。

CD では、main ブランチへ push したときや手動実行したときに apply を行いたいですが、Merge Request 作成/更新時は plan のみを確認できれば十分です。apply が MR の作成・更新ごとに実行されてしまうと、意図しないインフラ変更が発生するリスクがあります。

この課題では、Step 3（3-2、3-4）で学んだ `rules` と条件分岐を使って、MR 作成/更新時は apply しないで、main ブランチのときだけ apply を実行するようにします。

## プラクティス

`apply` job に、次の条件を満たす `rules` を追加してください。

条件は次のとおりです。

- `merge_request_event` のときは `apply` を実行しない
- `main` ブランチへの push / 手動実行時だけ `apply` を実行する
- main 以外のブランチから手動実行した場合は `apply` を実行しない

> ヒント:
>
> - `rules` は job レベルで定義できます
> - `if` 条件で `CI_PIPELINE_SOURCE` や `CI_COMMIT_BRANCH` を比較します
> - 複数の条件を組み合わせるときは、`&&` と `||` を使います

> 必要に応じて、次の公式ドキュメントを参照してください。
>
> - [GitLab CI/CD - rules](https://docs.gitlab.com/ci/yaml/#rules)
> - [GitLab CI/CD predefined variables](https://docs.gitlab.com/ci/variables/predefined_variables/)

## 確認

- main ブランチへの Merge Request を作成する
  - `plan` job は実行されることを確認する
  - `apply` job はスキップされることを確認する
- main ブランチに push する
  - `plan` job と `apply` job の両方が実行されることを確認する
- main 以外のブランチから手動実行する
  - `plan` job は実行されることを確認する
  - `apply` job はスキップされることを確認する

---

次のプラクティス：[5-4. apply 前に手動承認を挟む](./5-4-environment.md)
