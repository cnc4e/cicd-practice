# GitHub Actions 編

このセクションでは、GitHub Actions を使った CI/CD の構築を学びます。

まず workflow の基本構造を理解し、実行条件の制御や variables・secrets による値の管理方法を確認したうえで、実際に Terraform を使った CI/CD パイプラインを構築していきます。

すべての Step は [Step 1: 環境準備](./01-setup/README.md) が完了していることを前提とします。各 Step の課題は独立して取り組めるように設計しており、Step 1 の前提が満たせていれば順番を飛ばして実施できます。

ただし、Step 5 は Step 4 の内容（`terraform.yml` と plan artifact を扱う構成）を前提とします。

## 注意

既存の workflow や前の課題で作成した workflow が意図せず実行される場合があります。
課題の確認時に不要な workflow が同時に動かないよう、必要に応じて次のような対応を行ってください。

- 対象外の workflow を一時的にコメントアウトする
- 一時的に workflow ファイル名を変更する
- trigger 条件（on のイベントや対象ブランチ）を見直す

## ステップ一覧

| ステップ | タイトル                           | 内容                                                  | 想定所要時間 |
| -------- | ---------------------------------- | ----------------------------------------------------- | ------------ |
| Step 1   | [環境準備](./01-setup/README.md)   | GitHub リポジトリの作成・OIDC 設定                    | 30〜60分     |
| Step 2   | [基礎編](./02-basic/README.md)     | workflow の基本構造・`on` / `jobs` / `steps`          | 45〜60分     |
| Step 3   | [応用編](./03-advanced/README.md)  | 条件分岐・variables・secrets・artifact                | 60〜90分     |
| Step 4   | [実践編（CI）](./04-ci/README.md)  | Terraform の fmt / validate / plan を CI で自動化する | 60〜90分     |
| Step 5   | [実践編（CD）](./05-cd/README.md)  | Terraform apply を CD で実装する                      | 45〜60分     |
| Step 6   | [Runner 編](./06-runner/README.md) | GitHub-hosted runner / Self-hosted runner             | 30〜45分     |

---

まずは [Step 1: 環境準備](./01-setup/README.md) から始めてください。
