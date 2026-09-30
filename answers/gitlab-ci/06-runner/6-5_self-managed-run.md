# 解答例：6-5. Self-managed runner で pipeline を実行する

## 解答

6-2 で作成した `.gitlab-ci.yml` の job に `tags` を追加します。

```yaml
workflow:
  rules:
    - if: '$CI_PIPELINE_SOURCE == "web"'

runner-check:
  tags:
    - self-managed
  script:
    - hostname
    - whoami
    - cat /etc/os-release
    - echo $CI_RUNNER_ID
    - echo $CI_RUNNER_DESCRIPTION
```

## 解説

- `tags: [self-managed]` を追加するだけで、そのタグを持つ Self-managed runner 上で job が実行されます。タグの値は 6-4 で runner を登録したときに設定した値と一致させてください
- `hostname` の出力が登録したマシンのホスト名になっていることで、GitLab-hosted runner ではなく Self-managed runner で実行されていることを確認できます
- `CI_RUNNER_ID` の値は登録した runner 固有の ID になります。6-2 の実行結果と比較すると、異なる runner が使われていることがわかります
- `cat /etc/os-release` は、登録したマシンの OS 情報を出力します。6-2 の GitLab-hosted runner とは異なる環境であることを確認できます

---

[目次に戻る](../../README.md)
