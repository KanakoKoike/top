# Kanako Koike Website（GitHub Pages / Hugo）
[![Deploy Hugo site to GitHub Pages](https://github.com/KanakoKoike/top/actions/workflows/pages.yml/badge.svg)](https://github.com/KanakoKoike/top/actions/workflows/pages.yml)

このリポジトリは Hugo を用いた個人サイトの公開用リポジトリです。GitHub Actions による自動デプロイを設定しており、main ブランチにプッシュすると自動的に公開されます。低性能な LLM やツールでも操作を誤らないよう、極力手順を細かく記載します。

## 1. 仕組みの全体像（まずここだけ理解すればOK）

- ソース編集場所: `static/` 配下（`static/index.html`, `static/css/`, `static/js/`, `static/images/`）
- ビルド/公開: GitHub Actions が `hugo` でビルド → GitHub Pages に配信
- 本番URL: `https://kanakokoike.github.io/top/`
- 設定ファイル: `config.toml`（`baseURL` は上記URL）
- 生成物: リポジトリには含めません（`.gitignore` に `public/` と `docs/` を登録済）

## 2. リポジトリ構成（重要ファイル）

- `static/index.html` … サイトのトップページ（静的HTML）。ここを編集します。
- `static/css/style.css` … スタイル。
- `static/js/scroll.js` … JS。
- `static/images/` … 画像類。
- `config.toml` … Hugo の設定（`baseURL` が本番URL）。
- `.github/workflows/pages.yml` … 自動デプロイのワークフロー定義。
- `.gitignore` … 生成物（`public/`, `docs/`, `resources/`, `.hugo_build.lock`）を除外。

補足:
- `content/` や `layouts/` は使用していません（完全に静的サイト運用）。`static/` の中身がそのまま公開されます。

## 3. 日常の更新手順（最短手順）

1. 変更する
   - 例: `static/index.html` を編集、画像差し替えは `static/images/` に上書き。
2. ローカルで確認（任意）
   - コマンド例: `hugo server`（後述の「ローカル確認」参照）
3. コミットしてプッシュ
   - `git add -A`
   - `git commit -m "Update site content"`
   - `git push origin main`
4. デプロイの完了を確認
   - GitHub の Actions タブで `Deploy Hugo site to GitHub Pages` が Success になるのを確認。
   - 数分後に本番URLで表示確認。

## 4. ローカル確認（任意）

前提: Hugo（extended）をインストール済み。推奨バージョン: `v0.149.0` 近辺。

- サーバ起動: `hugo server`
  - ブラウザ: `http://localhost:1313/`
  - `static/` の内容がそのまま見えます。
- ビルドのみ: `hugo --minify`
  - 出力先は既定で `public/`（ただし `.gitignore` 済みなのでコミット不要）。

## 5. 自動デプロイの詳細（GitHub Actions → Pages）

- ワークフロー: `/.github/workflows/pages.yml`
  - トリガ: `main` ブランチへの push / 手動実行（workflow_dispatch）
  - 実行内容:
    1. リポジトリをチェックアウト
    2. Pages 用の設定を適用
    3. Hugo（extended）をセットアップ
    4. `hugo --minify -d ./public` でビルド
    5. `./public` を Pages アーティファクトとしてアップロード
    6. Pages にデプロイ
- GitHub のリポジトリ設定 → Pages の「Build and deployment」は「GitHub Actions」を選択してください。（既に切替済み）

## 6. よくある作業と注意点

- 画像の差し替え: 同名ファイルで `static/images/` に上書きするとリンク切れしにくいです。
- 新しいページを追加: `static/` 直下に `about.html` などを置き、`index.html` からリンクします。
- 絶対URL/相対URL: 画像やCSS/JSへのリンクは相対パス（例: `css/style.css`）で問題ありません。
- `baseURL` の更新: リポジトリ名やユーザ名を変更したら、`config.toml` の `baseURL` を必ず更新してください（末尾スラッシュ `/` を忘れない）。
- 生成物をコミットしない: `docs/` や `public/` をコミットしないでください（`.gitignore` 済み）。

## 7. トラブルシュート

- デプロイが反映されない
  - Actions タブで最新実行が Success か確認。失敗ならログの該当ステップを確認。
  - `baseURL` が本番URLと一致しているか確認（特に末尾の `/`）。
- `hugo` 実行時のエラー
  - `config.toml` が UTF-8（BOM なし）か確認。
  - Hugo が extended 版か確認。
- 404/レイアウト警告が出る
  - 本構成は `static/` のみ利用なので問題ありません。`content/` や `layouts/` 不要です。

## 8. 主要コマンド集（コピペ用）

```
# 変更を追加してプッシュ
git add -A
git commit -m "Update site content"
git push origin main

# ローカルプレビュー
hugo server

# ローカルビルド（生成物はコミット不要）
hugo --minify
```

---

メンテナ: Kanako Koike / 運用メモ最終更新: 2025-09
