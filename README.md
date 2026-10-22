# dev-trend-example

[sabas0ba/dev-trend](https://github.com/sabas0ba/dev-trend) を用いて、sabas0ba 配下の public リポジトリの開発トレンドレポートを定期生成し、GitHub Pages で公開するリポジトリ。ツール本体はツール側にあり、本リポジトリは設定、生成物、スナップショットを持つ。

公開先: https://sabas0ba.github.io/dev-trend-example/

## 構成

```
dev-trend.toml                      対象と出力の設定
.github/workflows/build-report.yml  週次の生成 workflow (ツール側の composite action を SHA 固定で呼ぶ)
docs/                               生成物 (index.html、report.txt、report.md)。workflow が commit し、Pages が配信する
data ブランチ                        snapshots/ (週次スナップショット) と summaries/ (要約結果)
```

`docs/` と data ブランチの内容は workflow が生成する。手で編集しない。

## 対象

`dev-trend.toml` の `owners` に指定した owner の public リポジトリ (archive 済みと fork を除く)。ツールは private リポジトリを対象にしない仕様であり、本リポジトリが public であることと整合する。レポートに含まれる PR のタイトル、コミットの件名、議論の抜粋は、いずれも public リポジトリ上で既に公開されている情報である。

## 実行

- 毎週月曜 00:00 UTC の schedule、main への push、手動実行 (Actions → Build report → Run workflow) で生成する
- 要約のエージェント step は手動実行時の入力 `summarize` で有効化する。手段は未設定であり、有効化しても通知だけを出す
- ツール側 `sabas0ba/dev-trend` の Settings → Actions → General → Access で、同一 owner のリポジトリからの利用を許可しておく必要がある

## Pages

Settings → Pages → Deploy from a branch、`main` の `/docs` を Source とする。`docs/.nojekyll` により Jekyll の処理を行わず、生成した HTML をそのまま配信する。

## 手元での生成

ツール側の clone にある script を、本リポジトリのディレクトリから呼ぶ。入出力は cwd からの相対で解釈される。

```bash
git fetch origin data && git worktree add .work/data data   # data ブランチを読む場合
<dev-trend の clone>/scripts/build-report.sh                  # 全段階。prepare / render で分割できる
```

手元実行では data ブランチと `docs/` を push しない。

## License

[Apache License 2.0](LICENSE)
