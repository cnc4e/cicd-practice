# 3-1. `include` で pipeline を分割して管理する

GitLab CI/CD では、`.gitlab-ci.yml` に pipeline の設定をすべて書くこともできますが、課題が増えるにつれてファイルが長くなり、管理しにくくなります。

`include` を使うと、別のファイルに書いた pipeline の定義を `.gitlab-ci.yml` に取り込めます。機能ごとにファイルを分けることで、pipeline の構成を整理しやすくなります。

Step 3 では、各課題で確認したい内容を `ci/` ディレクトリ以下のファイルに定義し、`.gitlab-ci.yml` から `include` で取り込む形で進めます。まずここで `include` の使い方を確認しておきましょう。

## プラクティス

次の条件を満たす構成を作成してください。

条件は次のとおりです。

- `ci/hello.gitlab-ci.yml` という新しいファイルを作成する
- そのファイルに、`echo "Hello from include"` を実行する job を 1 つ定義する
- `.gitlab-ci.yml` から `include` を使ってそのファイルを取り込む

`include` の書き方は次のとおりです。

```yaml
include:
  - local: ci/hello.gitlab-ci.yml
```

> ヒント:
>
> - `local` は同じリポジトリ内のファイルを参照するときに使います
> - 複数のファイルを取り込みたい場合は、リストに並べます
> - 取り込まれたファイルの job は、`.gitlab-ci.yml` に直接書いたのと同じように動作します

必要に応じて、次の公式ドキュメントを参照してください。

- [CI/CD YAML syntax reference - include](https://docs.gitlab.com/ci/yaml/#include)
- [include:local](https://docs.gitlab.com/ci/yaml/#includelocal)

## 確認

- 変更を push し、GitLab の CI/CD > Pipelines 画面で pipeline を開く
- `ci/hello.gitlab-ci.yml` に定義した job が実行されることを確認する
- `Hello from include` がログに出力されることを確認する

---

次のプラクティス：[3-2. `rules` で job の実行条件を分ける](./3-2-rules.md)
