# 6-5. Self-managed runner で pipeline を実行する

> **前提**: この課題は [6-4. Self-managed runner を導入する](./6-4-self-managed-setup.md) を完了していることを前提とします。また、Runner が **Online** 状態であることを確認してください。

Self-managed runner の登録が完了したら、実際に pipeline を実行して動作を確認します。

## プラクティス

6-2 で作成した `.gitlab-ci.yml` を修正して、Self-managed runner で実行されるようにしてください。

条件は次のとおりです。

- job に `tags` を追加し、6-4 で登録した runner のタグ（例: `self-managed`）を指定する
- それ以外の内容は変更しない

## 確認

- GitLab の **「Run pipeline」** ボタンから手動実行する
- pipeline が正常に完了することを確認する
- `hostname` の出力が、登録したマシンのホスト名であることを確認する
- 6-2 の実行結果と比較して、`CI_RUNNER_ID` や OS 情報の違いを確認する

> ヒント:
>
> - runner が停止している状態で pipeline を実行すると、job はキューに入ったまま待機します。runner を起動すると自動的に実行が始まります
> - `tags` に指定した値と runner に設定したタグが一致しないと job がキューから取り出されないため、タグの値を確認してください

## 後片付け

プラクティスが完了したら、次の順番で後片付けを行ってください。

### 1. Runner のアンインストール

登録を解除してから gitlab-runner をアンインストールします。

Linux（Ubuntu / Debian）の場合:

```bash
sudo gitlab-runner unregister --all-runners
sudo apt-get remove -y gitlab-runner
```

> **OS による差異**: Windows の場合は管理者権限で起動したコマンドプロンプト（または PowerShell）から次のように実行してください。
>
> ```cmd
> gitlab-runner unregister --all-runners
> gitlab-runner uninstall
> ```
>
> その他の OS のアンインストール手順は [Uninstall GitLab Runner](https://docs.gitlab.com/runner/install/) の各 OS セクションを参照してください。

### 2. GitLab 上での確認

**Settings > CI/CD > Runners** を開き、Runner が削除されていることを確認してください。残っている場合は画面から手動で削除してください。

- [Delete a runner](https://docs.gitlab.com/ci/runners/runners_scope/#delete-a-runner)

### 3. プロジェクトの公開範囲

引き続きプロジェクトを利用する場合は、必要に応じて Public に戻しても構いません。利用が完了したプロジェクトは削除するか Private のままにしておくことを検討してください。

必要に応じて、次の公式ドキュメントを参照してください。

- [Use tags to control which jobs a runner can run](https://docs.gitlab.com/ci/runners/configure_runners/#use-tags-to-control-which-jobs-a-runner-can-run)
- [Delete a runner](https://docs.gitlab.com/ci/runners/runners_scope/#delete-a-runner)

---

[Step 6 トップに戻る](./README.md)
