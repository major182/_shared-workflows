# _shared-workflows

複数のリポジトリで使う GitHub Actions の共通ワークフロー。

## claude-review（PR の自動レビュー）

PR を開いたとき・PR のブランチにプッシュしたときに、Claude Code が PR をレビューし、指摘を 1 つのコメントにまとめて投稿する（プッシュのたびに同じコメントを上書きする）。

- 下書きの PR はレビューしない。続けてプッシュしたときは古いレビューを止める
- フォークからの PR はレビューしない（claude-code-action の初期の設定）
- 指摘は「必須」「推奨」だけ。良い点・要約は書かない。指摘がなければ「指摘なし」の 1 行だけにする

### 使い方

1. 手元で `claude setup-token` を実行し、トークンを作る（Pro・Max プラン）
2. 使うリポジトリの Secret に登録する：`gh secret set CLAUDE_CODE_OAUTH_TOKEN --repo <owner>/<repo>`（値は貼り付けて入力する）
3. [examples/claude-review.yml](examples/claude-review.yml) を、そのリポジトリの `.github/workflows/` にコピーする
4. プロジェクト固有の観点は `with.extra_prompt` で足す

### 版の管理

呼び出す側は `@v1.0.0` のようにタグで版を固定する。レビューの指示を変えたら新しいタグを付け、呼び出す側の版を上げる。
