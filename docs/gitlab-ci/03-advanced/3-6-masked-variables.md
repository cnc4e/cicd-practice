# 3-6. masked variables で機密情報を扱う

機密情報（パスワードやAPIキーなど）は、絶対に pipeline ファイル内に直接書いてはいけません。なぜなら、リポジトリの履歴をさかのぼることで誰でも内容を見られてしまう危険があるためです。

GitLab CI/CD では、変数に **Masked** オプションを設定することで、値が実行ログに表示されないようにできます。また、**Protected** オプションを組み合わせると、Protected ブランチや Protected タグでのみ変数が参照できるようになります。

3-5 で扱った variables は機密情報の管理には向いていません。機密情報は必ず Masked を設定した variable で管理しましょう。

> 注意:
>
> Masked に設定した variable の値は、作成後に GitLab の画面から再表示できません。  
> 値を忘れた場合は、元のトークンやパスワードを再発行し、variable を上書きする必要があります。

## プラクティス

次の条件を満たす pipeline を `ci/3-6-masked-variables.gitlab-ci.yml` として新規に作成し、`.gitlab-ci.yml` から `include` で取り込んでください。

条件は次のとおりです。

- GitLab プロジェクトの CI/CD Variables に `SAMPLE_SECRET` という名前の variable を作成する
- 作成する variable に **Masked** を設定する
- pipeline の job でその variable を参照する
- `echo` で variable の値を出力する

> ヒント:
>
> - variable の Masked 設定は、Settings > CI/CD > Variables の追加・編集画面から設定できます
> - Masked 変数の値は 8 文字以上で、一部の特殊文字は使用できません

必要に応じて、次の公式ドキュメントを参照してください。

- [GitLab CI/CD variables - Mask a CI/CD variable](https://docs.gitlab.com/ci/variables/#mask-a-cicd-variable)
- [GitLab CI/CD variables - Protect a CI/CD variable](https://docs.gitlab.com/ci/variables/#protect-a-cicd-variable)

## 確認

- 変更を push し、pipeline を開く
- 実行ログに variable の値が `[MASKED]` としてマスクされて表示されることを確認する

---

次のプラクティス：[3-7. artifact を保存する](./3-7-artifact.md)
