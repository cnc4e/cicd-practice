# 3-5. variables を使って値を外部で管理する

GitLab CI/CD では、pipeline 内で使う値を変数として管理できます。

例えば「同じ pipeline でも環境（開発用・本番用など）によって対象や設定値を切り替えたい」ときに、変数を使うことで pipeline ファイル自体は共通のまま、値だけを外部から柔軟に変更できます。
（例：デプロイ先のサーバ名など、機密情報でない設定値の切り替えに便利です）

GitLab CI/CD の variables は、プロジェクトの Settings > CI/CD > Variables から管理でき、`$変数名` の形式で参照できます。

## プラクティス

次の条件を満たす pipeline を `ci/3-5-variables.gitlab-ci.yml` として新規に作成し、`.gitlab-ci.yml` から `include` で取り込んでください。

条件は次のとおりです。

- GitLab プロジェクトの CI/CD Variables に `SAMPLE_MESSAGE` という名前の variable を作成する
- pipeline の job でその variable を参照する
- `echo` で variable の値を出力する

> ヒント:
>
> - variable は GitLab のプロジェクト設定画面から追加・管理できます
> - pipeline の中では `$SAMPLE_MESSAGE` の形式で参照します

必要に応じて、次の公式ドキュメントを参照してください。

- [GitLab CI/CD variables](https://docs.gitlab.com/ci/variables/)
- [Add a CI/CD variable to a project](https://docs.gitlab.com/ci/variables/#add-a-cicd-variable-to-a-project)

## 確認

- 変更を push し、pipeline を開く
- 実行ログに variable の値が表示されることを確認する

---

次のプラクティス：[3-6. masked variables で機密情報を扱う](./3-6-masked-variables.md)
