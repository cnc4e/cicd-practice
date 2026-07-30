# Step 1: 環境準備

このステップでは、GitLab CI/CD の学習を始める前に必要な環境を準備します。

ここでは細かなインストール手順までは扱いません。各ツールの導入や設定は公式ドキュメントを参照し、まずは **ローカルでコードを編集して GitLab に push できること**、**GitLab CI/CD から AWS に接続できること** を確認してください。

## GitLab プロジェクトの作成

このプラクティスでは、GitLab CI/CD の pipeline を作成するための GitLab プロジェクトが必要です。

Step 2 以降では、このプロジェクトのリポジトリに `.gitlab-ci.yml` ファイルを追加し、push や Merge Request をきっかけに GitLab CI/CD を実行します。

このステップでは、作業用のプロジェクトを 1 つ用意してください。プロジェクトは **Private** で作成してください。

> **注意**: GitLab.com の Free プランでは、Private プロジェクトの CI/CD 実行時間は月 400 分までに制限されています。プラクティスを進めるうえで上限に達した場合は、プロジェクトを Public に変更するか、翌月を待ってください。
>
> また、機密情報（AWS のアクセスキーなど）は、**ファイルに書き込むこと自体を禁止します**。たとえ一時的であってもリポジトリにコミットしてはいけません。

- [プロジェクトを作成する](https://docs.gitlab.com/user/project/)

## GitLab への接続

このプラクティスでは、ローカルで作成したコードを GitLab に push できることを前提とします。

接続方法は HTTPS でも SSH でも構いませんが、継続して利用するなら SSH 接続を設定しておくと扱いやすいです。GitLab Docs には SSH キーの追加方法や接続確認の手順がまとまっています。

> **注意**: 社内ネットワークやプロキシ設定などで SSH 通信が制限されている環境では、SSH で GitLab に接続できない場合があります。その場合は HTTPS 接続を利用してください。

- [GitLab で SSH キーを使用する](https://docs.gitlab.com/user/ssh.html)
- [SSH 接続を確認する](https://docs.gitlab.com/ja-jp/user/ssh/#verify-your-ssh-connection)

## AWS 側の準備

このプラクティスでは、GitLab CI/CD から AWS に接続するために、IAM ユーザーのアクセスキーではなく OIDC を利用します。  
AWS 側では、まず GitLab の OIDC プロバイダーを IAM に登録し、その後、GitLab CI/CD から引き受けるための IAM ロールを作成してください。

### OIDC プロバイダーの作成

OIDC（OpenID Connect）は、GitLab CI/CD が AWS に安全に認証するための仕組みです。長期的なアクセスキーを GitLab に保存せず、実行時に発行される短時間のトークンを使って認証できます。

AWS マネジメントコンソールで IAM を開き、OIDC プロバイダーを作成してください。

設定する値は次のとおりです。

- プロバイダー URL: `https://gitlab.com`
- Audience: `sts.amazonaws.com`

> **注意**: GitLab.com 連携用の OIDC プロバイダー（`https://gitlab.com`）は、同じ AWS アカウント内で 1 つ作成されていれば利用できます。作成時に `already exists` エラーが出る場合は、既存の OIDC プロバイダーをそのまま利用してください。検証環境のように AWS アカウントを共用している場合は、このケースがよくあります。

参考:

- [AWSでOpenID Connectを設定して一時的な認証情報を取得する](https://docs.gitlab.com/ci/cloud_services/aws/)
- [Create an OpenID Connect (OIDC) identity provider in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html)

### 作成する IAM ロール

このステップでは、学習用リポジトリから AWS に接続するための IAM ロールを 1 つ作成してください。

ロール作成時は、**信頼されたエンティティタイプ** で **Web identity** を選択します。  
そのうえで、GitLab の OIDC プロバイダーと Audience を指定し、GitLab CI/CD から引き受けられるように設定してください。

想定するロールは次のようなものです。

- ロール名の例: `gitlab-ci-oidc-role`
- 用途: 学習用 GitLab プロジェクトから AWS に接続するためのロール
- 利用範囲: 少なくとも対象のプロジェクトに限定する
- 必要に応じて、対象ブランチまで絞る

### ロールに付与する権限

Step 1 では、GitLab CI/CD から AWS に認証できることを確認するために `aws sts get-caller-identity` を実行します。

また、後続のステップで Terraform が S3 バケットを作成するため、`s3:CreateBucket` 権限を付与してください。

参考:

- [GitLab CI/CD で OpenID Connect を使用して AWS に接続する](https://docs.gitlab.com/ci/cloud_services/aws/)
- [Create an OpenID Connect (OIDC) identity provider in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html)
- [Create a role for OpenID Connect federation (console)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-idp_oidc.html)

### 信頼ポリシーの考え方

信頼ポリシーでは、GitLab の OIDC プロバイダーを信頼し、少なくとも対象リポジトリだけがロールを引き受けられるように制限してください。GitLab の公式ドキュメントでも、`sub` クレームを使ってリポジトリや ref（ブランチ・タグ）を限定することが推奨されています。

GitLab CI/CD の `sub` クレームは次の形式です。

```text
project_path:NAMESPACE/PROJECT_NAME:ref_type:TYPE:ref:REF_NAME
```

プロジェクト全体を対象にする場合と、main ブランチだけに制限する場合の例は次のとおりです。

```text
# プロジェクト全体（すべてのブランチ・タグ）
project_path:myorg/cicd-practice:*

# main ブランチだけに制限する
project_path:myorg/cicd-practice:ref_type:branch:ref:main
```

信頼ポリシーの例（プロジェクト全体を対象にする場合）:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Federated": "arn:aws:iam::123456789012:oidc-provider/gitlab.com"
      },
      "Action": "sts:AssumeRoleWithWebIdentity",
      "Condition": {
        "StringEquals": {
          "gitlab.com:aud": "sts.amazonaws.com"
        },
        "StringLike": {
          "gitlab.com:sub": "project_path:myorg/cicd-practice:*"
        }
      }
    }
  ]
}
```

## サンプル pipeline の実行確認

環境準備ができたら、この章で作成した学習用リポジトリにサンプル pipeline を追加し、GitLab CI/CD から AWS に接続できることを確認してください。

ここで確認したいのは、次の 2 点です。

- GitLab に `.gitlab-ci.yml` を配置して pipeline を実行できること
- GitLab CI/CD から AWS に OIDC で認証できること

### 手順

1. 学習用リポジトリをローカルに clone する
2. リポジトリのルートにサンプル pipeline を `.gitlab-ci.yml` として作成する
3. ファイル内のプレースホルダを書き換える
    - `AWS_REGION`: 利用する AWS リージョン
    - `ROLE_ARN`: 作成した IAM ロールの ARN
4. ファイルを commit して push する
5. GitLab の CI/CD > Pipelines 画面で pipeline が成功していることを確認する
6. job のログで `aws sts get-caller-identity` の結果を確認する

### サンプル pipeline

以下の内容で、リポジトリのルートに `.gitlab-ci.yml` を作成してください。

```yaml
check-aws-oidc:
  image:
    name: amazon/aws-cli:latest
    entrypoint: [""]
  id_tokens:
    GITLAB_OIDC_TOKEN:
      aud: sts.amazonaws.com
  variables:
    AWS_REGION: <AWS_REGION>
    ROLE_ARN: <ROLE_ARN>
  script:
    - >
      aws sts assume-role-with-web-identity
      --role-arn "${ROLE_ARN}"
      --role-session-name "gitlab-ci-session"
      --web-identity-token "${GITLAB_OIDC_TOKEN}"
      --duration-seconds 3600
      > /tmp/assume-role-output.json
    - export AWS_ACCESS_KEY_ID=$(cat /tmp/assume-role-output.json | python3 -c "import sys, json; print(json.load(sys.stdin)['Credentials']['AccessKeyId'])")
    - export AWS_SECRET_ACCESS_KEY=$(cat /tmp/assume-role-output.json | python3 -c "import sys, json; print(json.load(sys.stdin)['Credentials']['SecretAccessKey'])")
    - export AWS_SESSION_TOKEN=$(cat /tmp/assume-role-output.json | python3 -c "import sys, json; print(json.load(sys.stdin)['Credentials']['SessionToken'])")
    - aws sts get-caller-identity
```

### 書き換える箇所

少なくとも次の 2 つを書き換えてください。

- `<AWS_REGION>`
  - 例: `ap-northeast-1`
- `<ROLE_ARN>`
  - 例: `arn:aws:iam::123456789012:role/gitlab-ci-oidc-role`

> **補足**: このサンプルでは `aws sts assume-role-with-web-identity` を直接呼び出してクレデンシャルを取得しています。Step 4 以降では、Terraform の AWS provider が OIDC トークンを使った認証をサポートしているため、より簡潔な構成に変更します。

## 確認事項

サンプル pipeline を push し、GitLab CI/CD が正常に実行されることを確認してください。

この pipeline が成功していれば、少なくとも次の前提が整っていることを確認できます。

- 学習用リポジトリを利用できる
- ローカルから GitLab に push できる
- GitLab CI/CD を実行できる
- AWS 側の OIDC ロール設定が正しい
- GitLab CI/CD から AWS に接続できる

> **注意**: ロール ARN には AWS アカウント ID が含まれます。確認後は ARN をプレースホルダ（<ROLE_ARN>）に戻してからコミットしてください。Step 4 以降では、ARN を GitLab CI/CD Variables に移して参照する構成に変更します。

## このステップのゴール

このステップのゴールは、GitLab CI/CD の pipeline を作成する前提となる GitLab プロジェクトと AWS 側の認証設定が整っていることです。

以降のステップでは、ここで準備した GitLab プロジェクトと AWS 連携設定を使って pipeline を作成し、GitLab CI/CD 上で CI/CD を実装していきます。

---

次のプラクティス：[Step 2: 基礎編](../02-basic/README.md)
