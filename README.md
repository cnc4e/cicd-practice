# CI/CD プラクティス

このリポジトリは、GitHub Actions と GitLab CI/CD を対象とした CI/CD 初学者向けの学習教材です。

Terraform と AWS を題材に、実際に workflow / pipeline を作成しながら、CI/CD の基本から実践的な構成までを段階的に学べます。

## 前提知識

このプラクティスは、次の知識があることを前提とします。

- Git の基本操作（[git-practice](https://github.com/cnc4e/git-practice)）
- YAML の基本的な読み書き
- Linux コマンドの基本
- GitHub または GitLab の基本的な操作
- Terraform の基礎的な概念（[terraform-practice](https://github.com/cnc4e/terraform-practice)）

GitHub Actions や GitLab CI/CD の細かな文法は、プラクティスの中で段階的に学ぶ前提です。

## 構成

教材は `docs/` ディレクトリに配置しています。模範解答は `answers/` ディレクトリに配置しています。

```text
docs/         # 課題本文
answers/      # 模範解答
terraform/    # プラクティスで使用する Terraform コード
```

---

## 目次

### 共通

GitHub Actions 編・GitLab CI/CD 編を始める前に、次の 2 つを順番に確認してください。

| #   | タイトル                               | 内容                                       |
| --- | -------------------------------------- | ------------------------------------------ |
| 1   | [CI/CD とは](./docs/00-overview.md)    | CI/CD の概要・CI と CD の違い・全体の流れ  |
| 2   | [環境準備（共通）](./docs/01-setup.md) | AWS アカウント・Git 環境など共通の前提準備 |

---

### [GitHub Actions 編](./docs/github-actions/README.md)

| ステップ | タイトル                                               | 内容                                                  |
| -------- | ------------------------------------------------------ | ----------------------------------------------------- |
| Step 1   | [環境準備](./docs/github-actions/01-setup/README.md)   | GitHub リポジトリの作成・OIDC 設定                    |
| Step 2   | [基礎編](./docs/github-actions/02-basic/README.md)     | workflow の基本構造・`on` / `jobs` / `steps`          |
| Step 3   | [応用編](./docs/github-actions/03-advanced/README.md)  | 条件分岐・variables・secrets・artifact                |
| Step 4   | [実践編（CI）](./docs/github-actions/04-ci/README.md)  | Terraform の fmt / validate / plan を CI で自動化する |
| Step 5   | [実践編（CD）](./docs/github-actions/05-cd/README.md)  | Terraform apply を CD で実装する                      |
| Step 6   | [Runner 編](./docs/github-actions/06-runner/README.md) | GitHub-hosted runner / Self-hosted runner             |

---

### [GitLab CI/CD 編](./docs/gitlab-ci/README.md)

| ステップ | タイトル                                        | 内容                                                  |
| -------- | ----------------------------------------------- | ----------------------------------------------------- |
| Step 1   | [環境準備](./docs/gitlab-ci/01-setup/README.md) | GitLab リポジトリの作成・OIDC 設定                    |
| Step 2   | 基礎編（準備中）                                | pipeline の基本構造・`stages` / `jobs`                |
| Step 3   | 応用編（準備中）                                | 条件分岐・variables・secrets・artifact                |
| Step 4   | 実践編（CI）（準備中）                          | Terraform の fmt / validate / plan を CI で自動化する |
| Step 5   | 実践編（CD）（準備中）                          | Terraform apply を CD で実装する                      |

---

## 模範解答

各プラクティスの模範解答は [answers/](./answers/README.md) に配置しています。

課題に取り組んでから参照することを推奨します。
