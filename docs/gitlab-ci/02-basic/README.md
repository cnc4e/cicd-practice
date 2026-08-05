# Step 2: 基礎編

> 前提: この課題は [Step 1: 環境準備](../01-setup/README.md) を完了していることを前提とします。

このステップでは、GitLab CI/CD の基本構造を理解し、最小の pipeline を自分で作成できるようになることを目指します。

GitLab CI/CD の pipeline は、リポジトリのルートに置いた `.gitlab-ci.yml` ファイルで定義します。pipeline には、実行単位である job と、job の中で順番に実行される `script` コマンドが含まれます。

このステップでは、次のような基本要素を順番に確認していきます。

- pipeline の作成
- pipeline の基本構造（job / `script`）
- `image` を用いた実行環境の指定
- `stages` を用いた job の実行順制御

まずは細かな書き方を覚えることよりも、push をきっかけに job が動き、その中で `script` が順番に実行されるという全体像をつかむことを意識してください。

## 2-1. pipeline を作成する

GitLab CI/CD では、`.gitlab-ci.yml` を書いて pipeline を定義します。

まずは最小の pipeline を作成し、pipeline の基本構造を確認します。

この段階では、まだ実行環境の指定は行いません。まずは job 名と `script` を持つ pipeline の骨組みを作ることを目的とします。

> **Note**
> Step 1 の動作確認で `.gitlab-ci.yml` を作成済みの場合は、そのファイルを上書きしてください。
> Step 1 のサンプル pipeline をそのまま残しておくと、push のたびに `check-aws-oidc` job が実行され続けます。

### プラクティス

以下のテンプレートを `.gitlab-ci.yml` として保存し、条件に合わせてカスタマイズしてください。

```yaml
<job 名>:
  script:
    - <コマンド>
```

条件は次のとおりです。

- ファイル名は `.gitlab-ci.yml` とする
- job 名は `hello` とする
- `echo "Hello, GitLab CI/CD"` を実行する

> **Note**
> この時点では `image` を指定していないため、runner の設定によっては job が失敗することがあります。
> pipeline 自体が起動していれば（CI/CD > Pipelines 画面に実行履歴が表示されていれば）正解です。

必要に応じて、次の公式ドキュメントを参照してください。

- [Tutorial: Create and run your first GitLab CI/CD pipeline](https://docs.gitlab.com/ci/quick_start/)
- [CI/CD YAML syntax reference](https://docs.gitlab.com/ci/yaml/)

**確認：**

- `.gitlab-ci.yml` が作成されていることを確認する
- 変更を push して、GitLab の CI/CD > Pipelines 画面に pipeline の実行履歴が表示されることを確認する

## 2-2. `image` で実行環境を指定する

GitLab CI/CD の job は、Docker イメージを実行環境として動作します。

その実行環境を指定するのが `image` です。`image` を書くことで、どの Docker イメージで job を実行するかを定義できます。

`image` を指定しない場合は runner のデフォルト設定が使われますが、明示的に指定することで、実行環境を固定して管理できます。

### プラクティス

前の課題で作成した pipeline に、以下の変更を加えてください。

- job に `image: alpine:latest` を追加する

> ヒント:
> 実行結果は GitLab の CI/CD > Pipelines 画面から job を開いて確認できます。

必要に応じて、次の公式ドキュメントを参照してください。

- [CI/CD YAML syntax reference - image](https://docs.gitlab.com/ci/yaml/#image)

**確認：**

- 変更を push して、pipeline が正常に完了することを確認する
- `Hello, GitLab CI/CD` が出力されることを確認する

## 2-3. 複数のコマンドを定義する

1 つの job の中には、`script` に複数のコマンドを定義できます。

`script` のコマンドは上から順番に実行されます。
job は処理のまとまり、`script` の各コマンドはその中で順番に実行される個々の処理、と考えると整理しやすいです。

### プラクティス

前の課題で作成した pipeline に、以下の変更を加えてください。

- `echo "Hello, GitLab CI/CD"` のコマンドを削除する
- 代わりに以下の 2 つのコマンドを定義する
  - 1 つ目で `echo "step1"` を実行する
  - 2 つ目で `echo "step2"` を実行する

必要に応じて、次の公式ドキュメントを参照してください。

- [CI/CD YAML syntax reference - script](https://docs.gitlab.com/ci/yaml/#script)

**確認：**

- 変更を push して、pipeline が正常に完了することを確認する
- step1 → step2 の順番で実行されることを確認する
- 両方の出力がログに表示されることを確認する

## 2-4. Merge Request をきっかけに pipeline を実行する

GitLab CI/CD はイベント起点で動きます。

デフォルトでは push をきっかけに pipeline が実行されますが、実際の CI では Merge Request をきっかけに検証を実行することがよくあります。

この課題では、`rules` を使って実行条件を制御し、**条件を変えると pipeline の動くタイミングも変わる**ことを確認します。

### プラクティス

前の課題で作成した pipeline に、以下の変更を加えてください。

- `rules` を使って、Merge Request のときだけ pipeline が実行されるようにする

> ヒント:
> GitLab CI/CD には `$CI_PIPELINE_SOURCE` という定義済み変数があり、pipeline の起動元を判定できます。Merge Request の場合、この変数の値は `"merge_request_event"` になります。
>
> main 以外のブランチで変更を push し、そのブランチから Merge Request を作成すると確認しやすいです。

必要に応じて、次の公式ドキュメントを参照してください。

- [CI/CD YAML syntax reference - rules](https://docs.gitlab.com/ci/yaml/#rules)
- [Predefined CI/CD variables reference](https://docs.gitlab.com/ci/variables/predefined_variables.html)

**確認：**

- main 以外のブランチで変更を push する
- push では pipeline が実行されないことを確認する
- Merge Request を作成して pipeline が実行されることを確認する
- CI/CD > Pipelines 画面でソースが `merge_request_event` になっていることを確認する

## 2-5. 複数の job を作る

pipeline は 1 つ以上の job から構成されます。

job を分けることで、処理を役割ごとに分離できます。

GitLab CI/CD では、同じ `stage` に属する job（または `stage` を指定していない job）は並列に実行されます。ここではまず、job を分けるとどうなるかを確認します。

### プラクティス

前の課題で作成した pipeline に、以下の変更を加えてください。

- `rules` を削除し、push で実行される状態に戻す
- job を 2 つに増やす
- 1 つ目の job で `echo "job1"` を実行する
- 2 つ目の job で `echo "job2"` を実行する
- 2 つの job に `stage` の指定はしない

> ヒント: どちらの job も同じ `image` を使って構いません。

必要に応じて、次の公式ドキュメントを参照してください。

- [CI/CD YAML syntax reference](https://docs.gitlab.com/ci/yaml/)

**確認：**

- 変更を push して、pipeline が正常に完了することを確認する
- CI/CD > Pipelines でパイプラインをクリックし、パイプラインのグラフ画面を開く
- 2 つの job が同じ列に並んで表示されていることを確認する（並列実行のサイン）
- それぞれの job をクリックし、ログに job1 / job2 が出力されていることを確認する

## 2-6. `stages` で job の実行順を制御する

GitLab CI/CD では、`stages` を使って job の実行順を定義できます。

`stages` にはステージ名を実行したい順番で並べ、各 job に `stage` を指定します。前のステージの job がすべて完了してから次のステージの job が実行されます。

この課題では、`stages` で job の実行順を制御し、実行順が変わることを確認します。

### プラクティス

前の課題で作成した pipeline に、以下の変更を加えてください。

- `stages` を定義し、2 つのステージを順番に並べる
- 1 つ目の job に `stage` を指定する
- 2 つ目の job に `stage` を指定する（1 つ目の後に実行されるよう）
- 1 つ目の job の `echo` を `echo "first job"` に変更する
- 2 つ目の job の `echo` を `echo "second job"` に変更する

必要に応じて、次の公式ドキュメントを参照してください。

- [CI/CD YAML syntax reference - stages](https://docs.gitlab.com/ci/yaml/#stages)
- [CI/CD YAML syntax reference - stage](https://docs.gitlab.com/ci/yaml/#stage)

**確認：**

- 変更を push して、pipeline が正常に完了することを確認する
- CI/CD > Pipelines でパイプラインをクリックし、パイプラインのグラフ画面を開く
- 2 つの job が左から右へ別々の列に表示されていることを確認する（ステージ順に並ぶ）
- 1 つ目の job が完了（緑のチェック）してから、2 つ目の job が開始されることを確認する
- それぞれの job をクリックし、ログに first job / second job が出力されていることを確認する

## このステップで押さえたいこと

このステップでは、GitLab CI/CD の基本を確認しました。

- pipeline はリポジトリルートの `.gitlab-ci.yml` で定義する
- pipeline はデフォルトで push をきっかけに起動する
- pipeline は 1 つ以上の job で構成される
- job の中では `script` のコマンドが順番に実行される
- `image` で job の実行環境を指定する
- `stages` と `stage` で job の実行順を制御する

次のステップでは、条件分岐や variables、artifacts を使って、pipeline をもう少し実践的な形にしていきます。

---

次のステップ：[Step 3: 応用編](../03-advanced/README.md)
