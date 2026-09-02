# 3-7. artifact を保存する

artifact は、pipeline 実行中に作成したファイルを保存するための仕組みです。

artifact を使うと、pipeline 実行後にファイルをダウンロードしたり、後続の job に引き渡したりできます。

この課題では、まずファイルを 1 つ保存できるようにするところまでを確認します。

CI/CD の運用では、artifact を無期限に残すとストレージ使用量が増え、コストや管理負荷が上がります。
また、古い成果物が長く残ると、参照ミスによって誤ったファイルを利用するリスクも高まります。
そのため、artifact は用途に応じて保持期間（`expire_in`）を明示的に設定することが重要です。

## プラクティス

次の条件を満たす pipeline を `ci/3-7-artifact.gitlab-ci.yml` として新規に作成し、`.gitlab-ci.yml` から `include` で取り込んでください。

条件は次のとおりです。

- 1 つ目の script でファイルを作成する
  - `result.txt` というファイルを作成する
  - `result.txt` の中に任意の文字列を書き込む
- `artifacts:paths` を使って `result.txt` を保存する
  - artifact の有効期限（`expire_in`）は `1 week` にする

> ヒント:
>
> - `artifacts:paths` にファイルのパスを列挙します
> - `artifacts:expire_in` を指定しない場合、プロジェクトの設定に依存します
> - 保持期間は「必ず参照が必要な成果物か」「失われても pipeline を再実行すれば新しく生成できるか」で判断すると運用しやすくなります

必要に応じて、次の公式ドキュメントを参照してください。

- [CI/CD YAML syntax reference - artifacts](https://docs.gitlab.com/ci/yaml/#artifacts)
- [Downloading job artifacts](https://docs.gitlab.com/ci/jobs/job_artifacts/)

## 確認

- 変更を push し、pipeline を開く
- job の実行結果画面に artifact が表示されることを確認する
- `result.txt` を artifact としてダウンロードできることを確認する

---

次のプラクティス：[3-8. job 間で値を受け渡す](./3-8-job-output.md)
