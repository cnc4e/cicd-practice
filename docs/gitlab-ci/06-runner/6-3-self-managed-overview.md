# 6-3. Self-managed runner とは何か

> **前提**: この課題は [6-2. GitLab-hosted runner で環境を確認する](./6-2-gitlab-hosted.md) を完了していることを前提とします。

GitLab-hosted runner は手軽に使える反面、環境のカスタマイズに制限があります。Self-managed runner を使うと、自分で用意した環境を runner として登録し、pipeline を実行できます。

## GitLab-hosted runner との比較

| 項目         | GitLab-hosted runner       | Self-managed runner             |
| ------------ | -------------------------- | ------------------------------- |
| 環境の用意   | GitLab が管理              | 自分で用意・管理                |
| 実行環境     | 毎回クリーンな VM          | 自分で用意したマシン            |
| OS           | Linux / Windows / macOS    | 任意（Linux / Windows / macOS） |
| カスタマイズ | 制限あり                   | 自由                            |
| コスト       | 無料枠あり、超過で従量課金 | マシンの維持コストが発生        |
| 管理コスト   | 不要                       | 利用者が負担                    |

## Self-managed runner の主な利用シーン

Self-managed runner は、次のような場合に使用されます。

- 社内ネットワーク内のリソースにアクセスする必要がある
- GPU など特定のハードウェアが必要
- 大規模ビルドや負荷の高いテストなど、より高い CPU / メモリ性能が必要（GitLab-hosted runner の標準スペックでは不足する）
- 独自のソフトウェアや設定が必要な環境で実行したい
- コスト面で GitLab-hosted runner の [プランごとの利用可能枠](https://docs.gitlab.com/ci/pipelines/compute_minutes/) を超えてしまう場合
- あるいは、GitLab 以外の実行基盤として AWS CodeBuild などを使う構成に切り分けたい場合

なお、実際のプロジェクトでは物理マシンや VM のほかに、Kubernetes 上で runner を管理する [GitLab Runner Operator](https://docs.gitlab.com/runner/install/operator/) や [GitLab Runner Helm Chart](https://docs.gitlab.com/runner/install/kubernetes/) が採用されることもあります。また、GitLab CI/CD だけでなく AWS CodeBuild のような外部実行基盤を併用する構成も実務では見られます。大規模な環境やコンテナ基盤を中心に構成されたプロジェクトでは、こうした選択肢も検討してみてください。

## セキュリティ上の注意点

**Self-managed runner を public プロジェクトに登録することは推奨されません。**

public プロジェクトでは、誰でも Merge Request を送ることができます。悪意のある MR が Self-managed runner 上で実行されると、そのマシンや社内ネットワークに意図しないコードが実行されるリスクがあります。

Self-managed runner を使う場合は、次の点を意識してください。

- **private プロジェクトでの利用を基本とする**
- runner を動かすマシンの権限を必要最小限にする
- 不要になった runner はすぐに削除する

このプラクティスでは、Step 1 で作成した学習用プロジェクトを **Private に変更したうえで** Self-managed runner を導入します。

## 事前準備：プロジェクトを Private に変更する

> すでに Private で作成している場合は、この手順は不要です。  
> プロジェクトの設定画面で公開範囲を確認し、Private であれば次の課題（6-4）に進んでください。

Public で作成している場合は、次の課題（6-4）に進む前に Private に変更してください。

- [プロジェクトの公開範囲を変更する](https://docs.gitlab.com/user/public_access/#change-project-visibility)

必要に応じて、次の公式ドキュメントを参照してください。

- [Self-managed runners](https://docs.gitlab.com/runner/)
- [Security for self-managed runners](https://docs.gitlab.com/ci/runners/configure_runners/#prevent-runners-from-revealing-sensitive-information)

---

次のプラクティス：[6-4. Self-managed runner を導入する](./6-4-self-managed-setup.md)
