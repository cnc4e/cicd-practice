# 解答例：6-2. GitLab-hosted runner で環境を確認する

## 解答

`.gitlab-ci.yml` の内容は以下のとおりです。

```yaml
workflow:
  rules:
    - if: '$CI_PIPELINE_SOURCE == "web"'

runner-check:
  script:
    - hostname
    - whoami
    - cat /etc/os-release
    - echo $CI_RUNNER_ID
    - echo $CI_RUNNER_DESCRIPTION
```

## 解説

- `workflow:rules` で `$CI_PIPELINE_SOURCE == "web"` を指定しているため、GitLab の「Run pipeline」ボタンからのみ pipeline が起動します
- `tags` を指定していないため、GitLab-hosted runner で実行されます
- `cat /etc/os-release` は OS のバージョン情報を出力します。GitLab-hosted runner が実際にどのディストリビューション・バージョンで動いているかを確認できます
- `CI_RUNNER_ID` は runner ごとに固有の ID です。複数回実行すると値が変わることで、毎回異なる runner（クリーンな VM）で実行されていることを確認できます
- `CI_RUNNER_DESCRIPTION` は runner の説明文です。GitLab-hosted runner では GitLab が自動的に設定した値が表示されます

---

[目次に戻る](../../README.md)
