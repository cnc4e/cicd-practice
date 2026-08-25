# Step 3: 応用編

> 前提: この課題は [Step 2: 基礎編](../02-basic/README.md) を完了していることを前提とします。

このステップでは、GitLab CI/CD の pipeline をもう少し実践的な形にしていきます。

Step 2 では pipeline の基本構造を扱いましたが、このステップでは **条件に応じて処理を切り替える方法**、**値を pipeline の外に出して扱う方法**、**成果物を保存する方法** を確認します。

このステップでは、次のような要素を扱います。

- **`include` による pipeline の分割管理**：pipeline 定義を複数ファイルに分けて管理しやすくします
- **`rules` による実行条件の制御**：job の実行条件を定義し、状況に応じて pipeline の動きを変えます
- **イベント・ブランチに応じた実行制御**：起動したイベントやブランチによって、実行する job を変えます
- **variables の利用**：機密情報でない設定値を pipeline の外で管理します
- **masked variables の利用**：パスワードやトークンのような機密情報を安全に扱います
- **job 間での値の受け渡し**：`artifacts` の dotenv 形式を使って、job 間で値を引き渡します
- **artifact の保存**：pipeline 実行中に作成したファイルを保存し、後からダウンロードできるようにします
- **`workflow:rules` による pipeline 全体の制御**：pipeline 自体を起動するかどうかを制御し、二重起動を防ぎます

> 進め方：
>
> 各課題では、その課題で確認したい内容だけを含む pipeline を別ファイルに定義し、`.gitlab-ci.yml` から `include` で取り込む方法で進めます。

## プラクティス一覧

| #   | タイトル                                                                         |
| --- | -------------------------------------------------------------------------------- |
| 3-1 | [`include` で pipeline を分割して管理する](./3-1-include.md)                     |
| 3-2 | [`rules` で job の実行条件を分ける](./3-2-rules.md)                              |
| 3-3 | [イベントによって実行する job を変える](./3-3-event-condition.md)                |
| 3-4 | [ブランチによって実行する job を変える](./3-4-branch-condition.md)               |
| 3-5 | [variables を使って値を外部で管理する](./3-5-variables.md)                       |
| 3-6 | [masked variables で機密情報を扱う](./3-6-masked-variables.md)                   |
| 3-7 | [artifact を保存する](./3-7-artifact.md)                                         |
| 3-8 | [job 間で値を受け渡す](./3-8-job-output.md)                                      |
| 3-9 | [`workflow:rules` で pipeline 全体の実行条件を制御する](./3-9-workflow-rules.md) |
