# 5-4. apply 前に手動承認を挟む

> **前提**: この課題は [5-3. apply の実行条件を制御する](./5-3-skip-on-pr.md) を完了していることを前提とします。

インフラへの変更は、意図したタイミングでのみ実行されるべきです。main への push をきっかけに自動で apply が走ると、想定外の変更がそのまま本番環境に反映されるリスクがあります。

この課題では、**無料プランでも承認の意図を体験できるように、手動ジョブで代替**します。`when: manual` を使って apply の前に確認を挟む構成を作り、plan を通過したあとに apply を安全に止める考え方を学びます。

この課題では、**手動承認用の job を追加して apply を止める**手順を行います。

> 補足:
>
> 本番運用では GitLab の Deployment approvals や Protected environments を使うと、より自然な承認フローを実現できます。ただし、無料プランでは利用できないため、この教材では `when: manual` を使った手動承認ジョブで代替します。

## プラクティス

以下の手順で、apply 前の承認フローを代用する構成を設定してください。

### 1. pipeline 定義に手動承認用 job を追加する

`apply` job の前に、手動承認用の job を追加してください。

条件は次のとおりです。

- `approve_apply` という job を追加する
- `when: manual` を設定し、手動で実行できるようにする
- `apply` job は `approve_apply` に依存するようにする

> ヒント:
>
> - `when: manual` を使うと、job が手動で承認待ち状態になります
> - `needs: ["plan", "approve_apply"]` のように依存関係を使うと、承認が完了してから apply を実行できます

### 2. Environment は参考情報として確認する

GitLab の Environment は、どの job がどの環境へデプロイしたかを管理するときに利用できます。
この課題では必須にしませんが、必要に応じて公式ドキュメントを確認してください。

> 必要に応じて、次の公式ドキュメントを参照してください。
>
> - [GitLab CI/CD - Environments](https://docs.gitlab.com/ci/environments/)
> - [GitLab CI/CD - jobs](https://docs.gitlab.com/ci/jobs/)
> - [GitLab CI/CD - when: manual](https://docs.gitlab.com/ci/yaml/#when)

## 確認

- main ブランチに push する（または UI から手動実行する）
- `plan` job が完了した後、`approve_apply` job が手動承認待ちの状態になることを確認する
- `approve_apply` job を手動で実行して承認したあと、`apply` job が実行されることを確認する

---

次のプラクティス：[5-5. plan 結果を利用して terraform apply を実行する](./5-5-apply.md)
