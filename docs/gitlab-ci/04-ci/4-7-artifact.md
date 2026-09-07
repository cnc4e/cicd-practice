# 4-7. plan 結果を artifact として保存する

> **前提**: この課題は [4-6. Merge Request で CI を実行する](./4-6-mr-trigger.md) を完了していることを前提とします。

4-5 で `terraform plan -out=tfplan` によって plan 結果をファイルとして保存しました。
ただし CI 実行環境は実行後に破棄されるため、そのままでは後から参照できません。

この課題では、Step 3（3-7）で学んだ artifact を使って、plan 結果を保存します。

> **補足: job 分割について**
>
> `tfplan` ファイルは `plan` を実行した job の実行環境にだけ存在します。
> そのため、`plan` と artifact 保存は同じ job の中で続けて実行します。

## プラクティス

`ci/terraform.gitlab-ci.yml` を編集し、次の条件を満たしてください。

条件は次のとおりです。

- `terraform plan` の結果ファイル `tfplan` を artifact として保存する
- artifact の有効期限（`expire_in`）を設定する
  - 例: `1 week`

> ヒント:
>
> - `artifacts:paths` に保存するファイルを指定します
> - `tfplan` は `terraform/` 配下に作成している場合、artifact のパスもそれに合わせて指定します

必要に応じて、次の公式ドキュメントを参照してください。

- [CI/CD YAML syntax reference - artifacts](https://docs.gitlab.com/ci/yaml/#artifacts)
- [Downloading job artifacts](https://docs.gitlab.com/ci/jobs/job_artifacts/)

## 確認

- 変更を push し、Merge Request または手動実行で pipeline を動かす
- `terraform plan` と artifact 保存が成功することを確認する
- job の実行結果画面に `tfplan` の artifact が表示されることを確認する

---

[Step 4 トップに戻る](./README.md)

次のステップ: Step 5: 実践編（CD）
