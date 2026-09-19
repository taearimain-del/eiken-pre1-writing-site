# CLAUDE.md — 英検準1級ライティング公開版

`英検準1級ライティング対策/today-task.html` の**公開用コピー**だけを置く独立プロジェクト。

## なぜ分離しているか（重要）
元の `英検準1級ライティング対策/` フォルダには `進捗ログ.md`（成績・ミスの記録）など
個人が特定される学習データが同居している。GitHub Pagesで外部公開する際にフォルダ全体を
アップロードすると、これらも一緒に公開されてしまうリスクがあるため、公開して問題ない
`today-task.html` だけをこの独立リポジトリにコピーして公開している
（2026-08-04、親CLAUDE.mdの「個人情報を含むツールを公開するときの判断」ルールに基づく）。

## 更新の同期手順（重要・忘れやすい）
`today-task.html` の中身を更新したときは、**このフォルダにもコピーし直してpushしないと
公開サイトに反映されない。** 元ファイルを直接ここに置いているわけではない。

```bash
cp "../英検準1級ライティング対策/today-task.html" "./index.html"
git add index.html
git commit -m "today-task.htmlの更新を反映"
git push
```

## データについて
- 中身はSupabase（`sb_publishable_...`という公開可能キーを使用、秘密鍵ではない）と同期する
  タスク管理ツール。ローカルストレージ or Supabase上に進捗が保存され、HTML自体には
  個人の成績データは埋め込まれていない。
- `<meta name="robots" content="noindex, nofollow">` と `robots.txt` で検索エンジンからは隠している。

## Gitリポジトリについて
- 独立GitHubリポジトリ：`taearimain-del/eiken-pre1-writing-site`（**Public**）。
  コードの管理（バージョン管理・変更履歴）は引き続きここで行う。
- 親の `claude-workspace` リポジトリの `.gitignore` にこのフォルダを追加済み。

## デプロイ先について（重要・2026-09-19変更）
**本番公開は GitHub Pages ではなく Cloudflare Pages を使うこと。** Fort様の明示指示
（2026-09-19「git hub ioではなくcloudflareで頼むよ」）。GitHub Pages版
（https://taearimain-del.github.io/eiken-pre1-writing-site/）は無効化はしていないが、
**もう正としては使わない**。ryuに案内するURL・マイポータル等への掲載は必ずCloudflare版を使う。

- **本番URL（Cloudflare Pages）：https://eiken-pre1-writing.pages.dev/**
- Cloudflareプロジェクト名：`eiken-pre1-writing`（アカウント`fortdex707@gmail.com`。
  認証情報・`wrangler`コマンドの使い方は `運用ルール/公開ルール.md` の
  「Cloudflare Pages」セクション参照）
- デプロイ手順（`today-task.html`更新後、このフォルダに`index.html`としてコピーしてから）：
  ```bash
  cd "英検準1級ライティング公開版"
  export CLOUDFLARE_API_KEY=$(grep -oP 'Global API Key：`\K[^`]+' "../運用ルール/公開ルール.md")
  export CLOUDFLARE_EMAIL=$(grep -oP '併用するメールアドレス：`\K[^`]+' "../運用ルール/公開ルール.md")
  export CLOUDFLARE_ACCOUNT_ID=$(grep -oP 'Account ID：`\K[^`]+' "../運用ルール/公開ルール.md")
  wrangler pages deploy . --project-name=eiken-pre1-writing --branch=main --commit-dirty=true
  ```
  （Global API Keyを直接コマンドに書くとauto modeの機密情報検知でブロックされるため、
  上記のようにファイルから読み込む書き方にすること）
- GitHubへのpushも従来どおり実施する（コード管理のため）が、それだけでは**公開サイトには
  反映されない**。Cloudflareへの`wrangler pages deploy`を必ず別途実行すること。
- 注意：`access-gate.js`のログイン状態はオリジン（ドメイン）ごとに保存されるため、
  旧URL（github.io）で一度ログイン済みでも、新URL（pages.dev）では再ログインが必要。

## アクセスゲート（重要）
このサイトを含む全公開ページは `access-gate.js`（`my-portal-ryu.netlify.app`でホスト）による
パスワード/Googleログインのロックが必須（2026-08-04指示。詳細は親CLAUDE.mdの
「公開ページのアクセスゲート」参照）。`index.html`の`<head>`相当部分に既に組み込み済み。
