# 当社の素材ライブラリ（仮の置き場所の雛形）

hp-core の `photo_search`・`photo_pick` が読む一覧（`index.json`）と写真の置き場所の雛形。形は `docs/design-layout.md` 6.1 章が正本。

## 方針（2026-09-25 にユーザーが決定）

- 入れる写真は**当社が権利を持つ物だけ**（自分で撮った物、権利を買い取った物）。フリー素材サイトの写真は入れない（規約の調査は `docs/research/stock-photos.html`）。
- `attribution_required` は常に `false`（帰属表示が要る写真は入れない）。
- 今入っている 4 枚はテスト用の無地の画像。実際の写真がそろったら差し替える。

## 仮の公開場所（W1b の K11 で使う）

GitHub Pages で配る。このフォルダは公開リポジトリ `wintermaples/hpmaton-photos` そのもので、HPMaton のリポジトリからは git の submodule（`photo-library/`）として使う。

1. リポジトリの Settings → Pages で、`main` ブランチの `/ (root)` を公開する（`.nojekyll` を置いてあるので、ファイルはそのまま配られる）。
2. ブラウザで `https://wintermaples.github.io/hpmaton-photos/index.json` が開けることを確かめる（JSON がそのまま表示される）。

hp-core の `packages/hp-core/server/design/photos.js` の `LIBRARY_URL` はこの URL になっている。別の場所で試すときは環境変数 `HP_PHOTO_LIBRARY` に一覧の URL を入れる。本番の置き場所は W2 までに決める（HPMaton の `docs/mvp-plan.md` 8b 章 Q24）。

写真の権利は当社にある。このリポジトリは HPMaton の仕組みで配るための物で、ほかの用途での利用は認めない。

## 写真を足すとき

1. `photos/<id>.jpg`（幅 2400 px 程度、2 MB 以下、JPEG）を置く。`id` は英小文字・数字・ハイフン（例：`kominka-02`）。
2. `thumbs/<id>.jpg`（幅 240 px）を作る：HPMaton のリポジトリで `node scripts/photo-library.js`（sharp で作り、`index.json` の `width`・`height`・`bytes` も直す。`.stage/linux/node_modules` の sharp を使うので、先に `bash scripts/pack.sh linux`）。
3. `index.json` の `photos` に項目を足す（`title`、`tags`、`purpose`（`hero` / `band` / `decor`）、`industry`、`source`、`author`、`license`）。`updated` を今日の日付に。
4. このフォルダの中で commit して push する（`git -C photo-library add -A && git -C photo-library commit -m "..." && git -C photo-library push`）。その後 HPMaton 側で `git add photo-library` と commit をして、submodule の指す版を進める。hp-core は一覧を 10 分だけ覚えるので、Claude デスクトップでは少し待つか hp-core を再起動する。
