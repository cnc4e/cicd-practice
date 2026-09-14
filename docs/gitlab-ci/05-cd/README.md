# Step 5: 実践編（CD）

> **前提**: このステップは [Step 4: 実践編（CI）](../04-ci/README.md) を完了していることを前提とします。

このステップでは、Step 4 で構築した CI をもとに、**Terraform の CD（継続的デリバリー）**を実装します。

CD では、検証済みの変更を安全に本番環境へ反映する仕組みを整えます。Terraform では、`terraform plan` の結果を使って `terraform apply` を実行する構成が代表的です。

このステップでは、次の観点を重視します。

- **手動承認を挟む**：apply を意図したタイミングでのみ実行する
- **main ブランチに限定する**：PR 時や非 main ブランチでは apply を行わない
- **CI の成果物を利用する**：Step 4 で保存した plan artifact を apply で使う

このステップでは、次のような要素を扱います。

- `workflow: rules`：main ブランチへの push と手動実行をトリガーとして扱う
- `needs`：plan と apply を分離し、順序を制御する
- `rules`：PR や非 main ブランチでは apply をスキップする
- GitLab の Environment / deployment approval：apply 前の承認を設定する
- `needs` と `artifacts: true`：plan job の artifact を apply job で取得する

> 進め方：
>
> このステップでは、Step 4 で作成した `ci/terraform.gitlab-ci.yml` を拡張しながら進めてください。  
> 5-1 以降は、既存の pipeline 定義を編集し、plan と deploy の役割を分けていく流れになります。  
> 最終的には、plan と apply が同じ pipeline に含まれる構成を作ります。
>
> **Step 4 をスキップした場合：**
>
> Step 5 から始める場合は、模範解答の [4-7. plan 結果を artifact として保存する](../04-ci/4-7-artifact.md) の完成形 pipeline を `.gitlab-ci.yml` や `ci/terraform.gitlab-ci.yml` としてコピーして、そこから 5-1 の課題に取り組んでください。

> **注意**: この教材では Step 4 のとおり local backend を前提にしています。GitLab runner は job ごとに実行環境が切り替わるため、同じ構成を複数回手動実行した場合、2 回目以降の `terraform apply` が失敗する場合があります。
>
> 再実行する場合は、事前に作成済みのリソースを手動で削除してから試してください。継続的な再実行を前提にする場合は、remote backend の利用を検討してください（この教材のスコープ外）。

## プラクティス一覧

| #   | タイトル                                                                  |
| --- | ------------------------------------------------------------------------- |
| 5-1 | [CD pipeline のトリガーを設定する](./5-1-trigger.md)                      |
| 5-2 | [plan と apply を job 分割する](./5-2-job-split.md)                       |
| 5-3 | [apply の実行条件を制御する](./5-3-skip-on-pr.md)                         |
| 5-4 | [GitLab の Environment で apply 前の承認を設定する](./5-4-environment.md) |
| 5-5 | [plan 結果を利用して terraform apply を実行する](./5-5-apply.md)          |

---

[Step 5 トップに戻る](./README.md)
