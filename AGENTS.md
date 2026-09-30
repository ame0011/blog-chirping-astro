## プロジェクト概要

[Chirping Astro](https://github.com/kannansuresh/chirping-astro) テーマを土台とした個人ブログ。
Astro 7 / Tailwind CSS v4 / daisyUI v5 / MDX / Pagefind。

- **パッケージマネージャは bun 固定**（`bun.lock` のみ存在。npm / pnpm / yarn は使わない）
- サイト設定（タイトル・ナビ・SNS・Giscus 等）はすべて `src/config.ts` に集約されている
- パスエイリアス: `~/` → `src/`、`@components/`、`@layouts/`、`@utils/`、`@styles/`、`@content/`、`@i18n/`

## 開発コマンド

```bash
bun run dev        # 開発サーバ (localhost:4321)
bun run build      # 本番ビルド + Pagefind インデックス生成
bun run preview    # ビルド結果のプレビュー
bun run lint       # ESLint (警告0)
bun run format     # Prettier で整形
bun run typecheck  # astro check
bun test           # ユニットテスト (bun test)
```

変更後は最低でも `bun run lint` と `bun run typecheck` を通すこと。

## 記事の作成・編集

### 配置

新規記事は必ず `src/content/posts/ja/` に `.md` または `.mdx` で作成する。

### frontmatter

`title`（1〜140文字）・`description`（1〜280文字）・`pubDate` は**必須**。
スキーマの定義は `src/content.config.ts` を参照。

```yaml
---
title: '記事タイトル'
description: '130文字程度の要約'
pubDate: 2026-09-30 # ISO 8601
updatedDate: 2026-10-01 # 任意
tags: [テクノロジー, Android]
categories: [] # 任意
heroImage: ../../../assets/images/posts/<slug>/thumbnail.png # 任意。md ファイルからの相対パス
heroImageAlt: '画像の代替テキスト' # heroImage を使う場合は必須
draft: false # true なら公開されない
pinned: false # 一覧の先頭に固定
toc: true # 目次を表示
math: false # 数式を使う場合 true
mermaid: false # Mermaid 図を使う場合 true
unlisted: false # 一覧・RSS・sitemap に出さない
---
```

- 外部 URL の画像は `astro.config.mjs` の `image.remotePatterns` に許可されたホストのみ最適化される
- `draft: true` 以外で未公開にしたい場合は `unlisted: true` を使う

### MDX

- コンポーネントは `import Image from '~/components/SmartImage.astro'` のように読み込む
- 独自コードフェンス `alert` が使える（`src/plugins/satteri-alert.ts`）

  ````markdown
  ```alert
  type: warning # info | success | warning | error
  style: soft # soft | outline | dash
  icon: lucide:triangle-alert
  title: 見出し
  description: 本文
  ```
  ````

  `description` は HTML エスケープされるため、Markdown 記法は解釈されない。

## コミットメッセージ

コミットメッセージは日本語とする。

`src/content/` 配下のファイルを含む場合は、記事の種類に応じて次の形式を使う。

| 対象           | 形式                       | 例                                                          |
| -------------- | -------------------------- | ----------------------------------------------------------- |
| 記事の追加     | `📝記事を作成 記事のタイトル`     | `📝記事を作成 Piko改造版のXアプリをNewXに…`                  |
| 既存記事の編集 | `✏記事を編集 記事のタイトル`     | `✏記事を編集 RadeonにてDDUを使用していて…`                  |
| 記事の削除     | `🗑記事を削除 記事のタイトル`     | `🗑記事を削除 古い下書き`                                     |

複数記事をまとめて扱う場合は「記事を作成」、内訳が混在する場合は最も支配的な変化に合わせる。

`src/content/` を含まない変更は、変更内容を簡潔にまとめた日本語の文で書く（`独自のフォントを削除し、ブラウザ指定のフォントで表示する`）。

## 禁止事項

- `.env` / `.env.local` / `.env.production` / `.env.development` を変更・コミットしない
  （`.env.example` は例外。環境変数を追加したときは `.env.example` も更新する）
- `git push` を行わない。`main` への push は `.github/workflows/deploy.yml` を起動して GitHub Pages へ即時公開するため、ユーザーの明示指示があるまで commit までで止める
- `bun.lock` を手動編集しない
- `dist/` / `.astro/` / `node_modules/` / `graphify-out/` をコミットしない
