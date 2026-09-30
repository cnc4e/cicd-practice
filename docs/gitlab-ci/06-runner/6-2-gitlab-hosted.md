# 6-2. GitLab-hosted runner で環境を確認する

> **前提**: この課題は [6-1. Runner とは何か](./6-1-runner.md) を完了していることを前提とします。

GitLab-hosted runner は、pipeline が実行されるたびに新しい仮想マシンとして起動します。この課題では、実際に job を実行してその環境を確認します。

GitLab-hosted runner の実態を把握しておくことで、「なぜ毎回 `terraform init` をやり直すのか」といった、これまでの課題で出てきた疑問の背景を理解できます。

## プラクティス

次の条件を満たす `.gitlab-ci.yml` を作成してください（最新の `main` から派生させた別ブランチで作業し、既存の `.gitlab-ci.yml` とは分けて確認することを推奨します）。

条件は次のとおりです。

- GitLab の **「Run pipeline」** ボタンから手動実行できるようにする（`$CI_PIPELINE_SOURCE == "web"` のときのみ実行されるよう `workflow:rules` で制御する）
- `tags` は指定しない（GitLab-hosted runner で実行されるようにする）
- 以下の情報を出力する job を作成する
  - ホスト名（`hostname` コマンド）
  - 実行ユーザー（`whoami` コマンド）
  - OS 情報（`cat /etc/os-release` コマンド）
  - 定義済み変数 `CI_RUNNER_ID` と `CI_RUNNER_DESCRIPTION` の値

## 確認

- GitLab の **「CI/CD > Pipelines > Run pipeline」** から手動で 2 回以上実行する
  - 実行ごとにホスト名が変わることを確認する
  - OS のバージョン情報を確認する
- `CI_RUNNER_ID` の値が実行のたびに変わることを確認する

> ヒント:
>
> - `CI_RUNNER_ID` などの定義済み変数は `script` 内で `echo $CI_RUNNER_ID` のように参照できます
> - ホスト名が毎回変わることが、「runner はクリーンな環境で起動する」ことを示しています
> - `workflow:rules` で `$CI_PIPELINE_SOURCE == "web"` を指定すると、「Run pipeline」ボタンからのみ pipeline が起動するようになります

必要に応じて、次の公式ドキュメントを参照してください。

- [GitLab-hosted runners](https://docs.gitlab.com/ci/runners/hosted_runners/)
- [Predefined CI/CD variables reference](https://docs.gitlab.com/ci/variables/predefined_variables/)

---

次のプラクティス：[6-3. Self-managed runner とは何か](./6-3-self-managed-overview.md)
