# 6-1. Runner とは何か

> **前提**: この課題は [Step 6: Runner 編](./README.md) の導入を読んでいることを前提とします。

これまでのステップでは、特に意識せずに pipeline が実行されてきました。この課題では、その裏側にある **Runner** の仕組みを整理します。

## Runner とは

Runner とは、GitLab CI/CD の job を実際に実行するマシンのことです。pipeline の各 job は、必ずいずれかの Runner 上で実行されます。

`.gitlab-ci.yml` の `tags` キーは、その job をどの Runner で実行するかを指定するものです。`tags` を省略した場合は、`tags` の指定がない job を受け付ける Runner が使われます。

```yaml
job-example:
  script:
    - echo "hello"
  tags:
    - self-managed  # このタグを持つ Runner 上で job が実行される
```

## Runner の種類

Runner には大きく 2 種類あります。

### GitLab-hosted runner

GitLab が管理・提供する runner です。pipeline が起動するたびに新しい仮想マシンが用意され、job 完了後に破棄されます。

主な特徴は次のとおりです。

- 利用者側でのセットアップは不要
- Linux / Windows / macOS を選択できる
- 毎回クリーンな環境で実行される
- 一定の無料枠があり、超過すると従量課金になる（GitLab.com の場合）

`tags` を指定しない job や、`saas-linux-small-amd64` などの GitLab 提供のタグを指定した job が GitLab-hosted runner で実行されます。

### Self-managed runner

利用者が自分で用意・管理する runner です。自社サーバーやクラウド上の VM など、任意のマシンを runner として登録できます。

主な特徴は次のとおりです。

- 専用のハードウェアや特定の環境が必要な場合に使用する
- 実行環境を自由にカスタマイズできる
- GitLab への通信が必要（アウトバウンド）
- 管理・運用コストは利用者が負担する

登録時に付与したタグを `tags` で指定することで、特定の Self-managed runner に job をルーティングできます。

## このステップで学ぶこと

次の 6-2 では GitLab-hosted runner の実際の環境を確認します。6-3 以降では Self-managed runner の概念・導入・実行を順に進めます。

必要に応じて、次の公式ドキュメントを参照してください。

- [GitLab-hosted runners](https://docs.gitlab.com/ci/runners/hosted_runners/)
- [Self-managed runners](https://docs.gitlab.com/runner/)

---

次のプラクティス：[6-2. GitLab-hosted runner で環境を確認する](./6-2-gitlab-hosted.md)
