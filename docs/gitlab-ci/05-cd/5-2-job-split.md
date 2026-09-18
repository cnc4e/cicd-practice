# 5-2. plan と apply を job 分割する

> **前提**: この課題は [5-1. CD pipeline のトリガーを設定する](./5-1-trigger.md) を完了していることを前提とします。

Step 4 では `terraform plan` までを 1 つの job にまとめていました。CD では `apply` を別の job として分離します。

job を分けることで、plan の結果を確認したうえで承認やスキップを挟めるようになります。この課題では、まず **job の分割だけ**を行います。承認フローや条件制御は後続の課題で実装します。

この課題では、Step 2 で学んだ複数 job と Step 3（3-2）で学んだ `needs` の知識を活かして、既存の job を `plan` と `apply` に分割します。

## プラクティス

`ci/terraform.gitlab-ci.yml` の job 構成を、次の条件を満たすように変更してください。

条件は次のとおりです。

- 既存の job を `plan` job に名前を変更する
  - 内容はそのまま変更しない
  - `stage: test` を追加する
- `apply` job を新たに追加する
  - `needs` を使って `plan` job の完了後に実行されるようにする
  - `stage` は `deploy` にする
  - `script` には `echo "apply"` を 1 行だけ実行するように定義する

> ヒント:
>
> - job を分割するときは、`plan` と `apply` で `stage` を役割に合わせて設定します
> - `needs` の使い方は Step 3（3-2）を参照してください

> 必要に応じて、次の公式ドキュメントを参照してください。
>
> - [GitLab CI/CD - Jobs](https://docs.gitlab.com/ci/jobs/)
> - [GitLab CI/CD - needs](https://docs.gitlab.com/ci/yaml/#needs)

## 確認

- 変更を push し、GitLab の UI から pipeline を手動実行する
- `plan` job が完了した後、`apply` job が実行されることを確認する
- `plan` job の `fmt` / `validate` / `plan` が正常に完了することを確認する
- `apply` job で `echo "apply"` が実行されることを確認する

---

次のプラクティス：[5-3. apply の実行条件を制御する](./5-3-skip-on-pr.md)
