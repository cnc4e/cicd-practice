# GitLab CI/CD 編

このセクションでは、GitLab CI/CD を使った CI/CD の構築を学びます。

まず pipeline の基本構造を理解し、実行条件の制御や variables・secrets による値の管理方法を確認したうえで、実際に Terraform を使った CI/CD パイプラインを構築していきます。

すべての Step は [Step 1: 環境準備](./01-setup/README.md) が完了していることを前提とします。各 Step の課題は独立して取り組めるように設計しており、Step 1 の前提が満たせていれば順番を飛ばして実施できます。

ただし、Step 5 は Step 4 の内容（`terraform.gitlab-ci.yml` と plan artifact を扱う構成）を前提とします。

## 注意

既存の pipeline や前の課題で作成した pipeline が意図せず実行される場合があります。
課題の確認時に不要な pipeline が同時に動かないよう、必要に応じて次のいずれかの対応を行ってください。

- 対象外の job を一時的にコメントアウトする
- `rules` や `only` / `except` で実行条件を絞る
- `.gitlab-ci.yml` の `include` で参照しているファイルを一時的に除外する
- `.gitlab-ci.yml` を別のファイル名（例: `.gitlab-ci.yml.bak`）に一時的にリネームする（GitLab は `.gitlab-ci.yml` 以外のファイルを CI 設定として読み込まないため、pipeline 自体が起動しなくなります）

## ステップ一覧

| ステップ | タイトル                          | 内容                                                  |
| -------- | --------------------------------- | ----------------------------------------------------- |
| Step 1   | [環境準備](./01-setup/README.md)  | GitLab プロジェクトの作成・OIDC 設定                  |
| Step 2   | [基礎編](./02-basic/README.md)    | pipeline の基本構造・`stages` / `jobs` / `script`     |
| Step 3   | [応用編](./03-advanced/README.md) | 条件分岐・variables・secrets・artifact                |
| Step 4   | [実践編（CI）](./04-ci/README.md) | Terraform の fmt / validate / plan を CI で自動化する |
| Step 5   | [実践編（CD）](./05-cd/README.md) | Terraform apply を CD で実装する                      |

---

まずは [Step 1: 環境準備](./01-setup/README.md) から始めてください。
