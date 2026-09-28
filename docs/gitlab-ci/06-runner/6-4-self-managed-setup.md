# 6-4. Self-managed runner を導入する

> **前提**: この課題は [6-3. Self-managed runner とは何か](./6-3-self-managed-overview.md) を完了していることを前提とします。また、学習用プロジェクトを **Private に変更済み**であることを確認してください。

この課題では、自分のマシンを Self-managed runner として GitLab プロジェクトに登録します。

## 事前準備：Runner を動かすマシンを用意する

Runner を登録するマシンを事前に用意してください。利用できる環境であれば何でも構いません。

例:

- ローカル PC（Linux / macOS / Windows）
- AWS EC2 インスタンス（例: Amazon Linux 2023 / Ubuntu）

EC2 を使う場合は、GitLab への**アウトバウンド通信（HTTPS / ポート 443）**が許可されていることを確認してください。runner は GitLab に対してポーリングして job を受け取るため、インバウンドの穴あけは不要です。

## 導入手順

### 1. GitLab の設定画面を開く

学習用プロジェクトの **Settings > CI/CD > Runners** を開き、**New project runner** をクリックしてください。

### 2. Runner の設定を入力する

次の項目を入力してください。

- **Tags**: runner を識別するタグを入力します（例: `self-managed`）。job の `tags` でこの値を指定することで、この runner が選択されます
- **Run untagged jobs**: タグのない job も受け付ける場合はチェックします。このプラクティスでは**チェックしない**でください
- **Description**: 任意の説明（省略可）

設定が完了したら **Create runner** をクリックしてください。

**Create runner** をクリックすると、次のステップで使う **authentication token**（`glrt-` で始まる文字列）が画面に表示されます。このトークンはこの画面を閉じると再表示されないため、必ずコピーしておいてください。

### 3. Runner の登録コマンドを実行する

画面に `gitlab-runner register` コマンド（authentication token 付き）が表示されます。Runner を動かすマシンで次の手順を実施してください。

#### gitlab-runner のインストール

Linux（Ubuntu / Debian）の場合:

```bash
curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash
sudo apt-get install -y gitlab-runner
```

その他の OS のインストール手順は、画面の **「How to install GitLab Runner」** リンクを参照してください。

#### Runner の登録

画面に表示されたコマンドをそのまま実行してください。コマンドの形式は次のとおりです。

```bash
sudo gitlab-runner register \
  --url https://gitlab.com \
  --token <authentication-token>
```

> **OS による差異**: 上記は Linux / macOS 向けのコマンドです。Windows の場合は `sudo` が不要で、管理者権限で起動したコマンドプロンプト（または PowerShell）から次のように実行してください。
>
> ```cmd
> gitlab-runner register --url https://gitlab.com --token <authentication-token>
> ```
>
> Windows への gitlab-runner インストール手順は [Install GitLab Runner on Windows](https://docs.gitlab.com/runner/install/windows/) を参照してください。

実行中に次の項目を入力するプロンプトが表示されます。

- **GitLab instance URL**: そのまま Enter（コマンド引数で指定済み）
- **Verification token**: そのまま Enter（コマンド引数で指定済み）
- **Runner description**: 任意の名前（省略時はホスト名が使われます）
- **Tags**: そのまま Enter（GitLab UI で設定済み）
- **Maintenance note**: そのまま Enter
- **Executor**: `shell` と入力してください（Docker 環境がある場合は `docker` も選択できます）

登録が完了すると、runner はサービスとして自動的に起動します。

### 4. Runner の起動を確認する

gitlab-runner サービスが起動していることを確認してください。

```bash
sudo gitlab-runner status
```

`gitlab-runner: Service is running` と表示されれば正常に起動しています。

## 確認

- GitLab の **Settings > CI/CD > Runners** を開く
- 登録した runner が **Online** 状態で表示されていることを確認する

> ヒント:
>
> - `gitlab-runner register` を実行した後は、サービスが自動起動します。別途 `run.sh` などの起動コマンドは不要です
> - EC2 を使用している場合、再起動後も runner が自動起動するかを確認しておくと安心です（`systemctl is-enabled gitlab-runner` で確認できます）

必要に応じて、次の公式ドキュメントを参照してください。

- [Install GitLab Runner](https://docs.gitlab.com/runner/install/)
- [Register a runner](https://docs.gitlab.com/runner/register/)

---

次のプラクティス：[6-5. Self-managed runner で pipeline を実行する](./6-5-self-managed-run.md)
