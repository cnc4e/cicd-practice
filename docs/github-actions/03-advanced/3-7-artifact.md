# 3-7. artifact を保存する

artifact は、workflow 実行中に作成したファイルを保存するための仕組みです。

artifact を使うと、workflow 実行後にファイルをダウンロードしたり、別の job に引き渡したりできます。

この課題では、まずファイルを 1 つ保存できるようにするところまでを確認します。

CI/CD の運用では、artifact を無期限に残すとストレージ使用量が増え、コストや管理負荷が上がります。
GitHub Actions では `retention-days` で保持期間を指定できます。「必ず参照が必要な成果物か」「失われても workflow を再実行すれば新しく生成できるか」を判断して、適切な期間を設定することが重要です。

## プラクティス

次の条件を満たす workflow を `3-7-artifact.yml` として新規に作成してください。

条件は次のとおりです。

- workflow の起動条件は `workflow_dispatch` にする
- 1 つ目の step でファイルを作成する
	- `result.txt` というファイルを作成する
	- `result.txt` の中に任意の文字列を書き込む
- 2 つ目の step で artifact として保存する
	- `upload-artifact` を使って `result.txt` を保存する
	- artifact の名前は `result` にする

> ヒント:
>
> - `upload-artifact` は GitHub が公式に提供する Action です
> - `uses` を使って呼び出し、`with` で保存先などのパラメータを指定します

> 必要に応じて、次の公式ドキュメントを参照してください。
>
> - [About workflow artifacts](https://docs.github.com/en/actions/concepts/workflows-and-actions/workflow-artifacts)
> - [Store and share data with workflow artifacts](https://docs.github.com/en/actions/tutorials/store-and-share-data)
> - [Downloading workflow artifacts](https://docs.github.com/actions/managing-workflow-runs/downloading-workflow-artifacts)

## 確認

- 変更を push し、Actions 画面から workflow を手動実行する
- Actions の実行結果画面に artifact が表示されることを確認する
- `result.txt` を artifact としてダウンロードできることを確認する

---

[Step 3 トップに戻る](./README.md)

次のステップ：[Step 4: 実践編（CI）](../04-ci/README.md)
