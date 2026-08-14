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
- 独立GitHubリポジトリ：`taearimain-del/eiken-pre1-writing-site`（**Public**、GitHub Pages公開用）
- 親の `claude-workspace` リポジトリの `.gitignore` にこのフォルダを追加済み。

## デプロイ（GitHub Pages）
- 本番URL：https://taearimain-del.github.io/eiken-pre1-writing-site/
- 設定：`main`ブランチのルートから配信。再pushすれば自動的に再ビルドされる。

## アクセスゲート（重要）
このサイトを含む全公開ページは `access-gate.js`（`my-portal-ryu.netlify.app`でホスト）による
パスワード/Googleログインのロックが必須（2026-08-04指示。詳細は親CLAUDE.mdの
「公開ページのアクセスゲート」参照）。`index.html`の`<head>`相当部分に既に組み込み済み。
