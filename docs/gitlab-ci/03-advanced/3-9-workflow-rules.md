# 3-9. `workflow:rules` で pipeline 全体の実行条件を制御する

> **前提**: この課題は [3-3. イベントによって実行する job を変える](./3-3-event-condition.md) を完了していることを前提とします。

3-3 では job 単位で `rules` を使って実行条件を制御しました。しかし、push と Merge Request の両方をトリガーにしている場合、同じコミットに対して pipeline が 2 回起動することがあります。

GitLab CI/CD には `workflow:rules` という仕組みがあり、**pipeline 全体を起動するかどうか**をファイルのトップレベルで制御できます。job ごとに `rules` を書かなくても、pipeline 自体を起動しないようにできます。

実務では「Merge Request のときだけ pipeline を起動する、push のみのときは起動しない」という制御によって、不要な pipeline の二重起動を防ぐことがよくあります。

## プラクティス

次の条件を満たす pipeline を `ci/3-9-workflow-rules.gitlab-ci.yml` として新規に作成し、`.gitlab-ci.yml` から `include` で取り込んでください。

条件は次のとおりです。

- `workflow:rules` を使って pipeline 全体の実行条件を定義する
- Merge Request のときは pipeline を起動する
- main ブランチへの push のときは pipeline を起動する
- それ以外のブランチへの push では pipeline を起動しない
- pipeline が起動したとき、`echo "pipeline started"` を実行する job を 1 つ定義する

> ヒント:
>
> - `workflow:rules` はファイルのトップレベルに書きます（job の外側）
> - `workflow:rules` の構文は job の `rules` と同じ書き方が使えます
> - 条件に合致しない場合、pipeline 自体が作成されません

必要に応じて、次の公式ドキュメントを参照してください。

- [Control pipeline with workflow rules](https://docs.gitlab.com/ci/yaml/workflow/)
- [CI/CD YAML syntax reference - workflow:rules](https://docs.gitlab.com/ci/yaml/#workflowrules)

## 確認

- main 以外のブランチに変更を push し、CI/CD > Pipelines 画面を開く
- pipeline が起動されていないことを確認する
- Merge Request を作成し、pipeline が起動されることを確認する
- main ブランチに直接 push（またはマージ）し、pipeline が起動されることを確認する

---

[Step 3 トップに戻る](./README.md)

次のステップ：[Step 4: 実践編（CI）](../04-ci/README.md)
