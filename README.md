# Kanako Koike Website — GitHub Pages / Hugo

[![Deploy Hugo site to GitHub Pages](https://github.com/KanakoKoike/top/actions/workflows/pages.yml/badge.svg)](https://github.com/KanakoKoike/top/actions/workflows/pages.yml)

このリポジトリは、静的ページを優先しつつ、将来的な Hugo 運用にも対応できる構成です。GitHub Actions による自動デプロイを設定しており、`main` ブランチへプッシュすると数分で公開されます。

## 1. 運用方針（静的優先 + Hugo対応）
- 現在の本番は静的ファイル（`static/index.html`）を優先して表示します。
- Hugo テーマの解決に失敗しても、トップは常に安定表示されます。
- 将来 Hugo に切り替える場合は、`static/index.html` をリネーム/削除すれば、`content/_index.md` を元にテーマ出力へ移行できます。

## 2. 公開URL / 主要ファイル
- 公開URL: `https://kanakokoike.github.io/top/`
- 静的トップ: `static/index.html`
- スタイル: `static/css/style.css`
- 画像: `static/images/`
- Hugo用トップ: `content/_index.md`
- 設定: `config.toml`
- デプロイ: `.github/workflows/pages.yml`

## 3. 更新手順（最短）
1) 変更（例: `static/index.html`/画像差し替え）
2) コミット/プッシュ
```
git add -A
git commit -m "Update site content"
git push origin main
```
3) 数分後、公開URLで確認

## 4. Hugo での確認（任意）
- ローカルでテーマを使うには Go + Hugo(extended) が必要。
- 実行: `hugo server` → `http://localhost:1313/`
- CI では Go / Hugo を自動セットアップしてビルドします。

## 5. 注意点
- 生成物（`public/`, `docs/`, `resources/`）はコミット不要です。
- `baseURL` は `config.toml` に設定済み（末尾の `/` を維持）。
- 画像差し替えは同名上書きが簡便です（`static/images/`）。

---

Maintainer: Kanako Koike / Last updated: 2025-09
