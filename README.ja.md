# @fogtype/css

[English](README.md) | **日本語**

タイポグラフィと余白だけで構造を伝える、ミニマルなクラスレスCSSフレームワーク。

セマンティックなHTML（`<h1>` / `<p>` / `<table>` / `<form>` …）がそのまま整ったレイアウトになります。CJKと欧文の混植を前提とした CJK向けの設計で、`text-autospace` による CJK・欧文間スペースの自動挿入、`rlh`（root line-height）基準の余白リズム、影を使わないフラットな表現を採用しています。

## Overview

- **読みやすさ** - 本文幅を `ric`（文字幅基準）で制限してブレークポイントなしで全幅に適応、影を使わないフラットな表現
- **クラスレス** - 素のHTMLがそのまま整う。スタイルは `:root` のCSS変数を上書きするだけで差し替え可能（1ファイル・依存関係ゼロ）
- **CJK対応** - `text-autospace` でCJK・欧文間スペースを、`text-spacing-trim` で約物の余白を自動調整

実際の表示は[プレビュー](https://kou029w.github.io/css/preview.ja.html)で確認できます。

## Usage

### HTML（CDN）

`<head>` に1行追加するだけです。

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@fogtype/css" />
```

あとはセマンティックなHTMLを記述します。

```html
<!doctype html>
<html lang="ja" dir="ltr">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@fogtype/css" />
  </head>
  <body>
    <h1>見出し</h1>
    <p>本文はクラスなしで整います。日本語とEnglishの混植も自動調整されます。</p>
  </body>
</html>
```

### NPM

```sh
npm install @fogtype/css
```

バンドラ経由で取り込む場合:

```css
@import "@fogtype/css";
```

```js
import "@fogtype/css";
```

ローカルにコピーして使う場合は [`index.css`](./index.css) をそのまま配置します。依存関係はありません。

## Themes

テーマはすべて **`:root` の CSS 変数（カスタムプロパティ）として宣言** されています。色・タイポグラフィ・余白・角丸はこの変数群を起点にしており、変数を上書きするだけでテーマ全体を差し替えられます。

変数は次の4つのグループに分かれます。

| グループ   | プレフィックス                | 役割                                                |
| ---------- | ----------------------------- | --------------------------------------------------- |
| Color      | `--color-*`                   | ブランド / 意味 / ニュートラルの全色                |
| Typography | `--text-*` / `--leading-*`    | 文字サイズと行間                                    |
| Spacing    | `--space-*` / `--line-length` | block（`rlh`）・inline（`rem`）の余白リズムと本文幅 |
| Shape      | `--radius`                    | 操作要素の角丸                                      |

各変数の既定値は後述の [Legend - default theme (`:root`)](#legend-default-theme-root) を参照してください。

## Customizing

独自の CSS で `:root`（または任意のスコープ）の変数を上書きします。フレームワーク本体（`index.css`）を読み込んだ **後** に置くのがポイントです。

<!-- prettier-ignore-start -->
```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@fogtype/css" />
<style>
  :root {
    --color-primary: oklch(54% 0.247 293);      /* ブランドカラー差し替え */
    --color-primary-dark: oklch(40% 0.247 293); /* ホバー時 */
    --radius: 0;                                /* 角張らせる */
    --line-length: 50;                          /* 本文幅を 50ric に広げる */
  }
</style>
```
<!-- prettier-ignore-end -->

ページの一部だけテーマを変えたい場合は、任意の要素にスコープして変数を再宣言できます。

```css
.brand-section {
  --color-primary: oklch(52% 0.14 148);
  --color-background: oklch(96% 0.006 148);
}
```

## Legend - default theme (`:root`)

`index.css` の `:root` で宣言されている既定テーマの全トークンです。

### Color

| 変数                     | 既定値                 | 用途                                   |
| ------------------------ | ---------------------- | -------------------------------------- |
| `--color-primary`        | `oklch(52% 0.165 255)` | ブランドカラー、リンク                 |
| `--color-primary-dark`   | `oklch(44% 0.168 255)` | ホバー・プレス時のプライマリー         |
| `--color-primary-light`  | `oklch(88% 0.045 255)` | 選択行・淡いハイライト（`mark` 等）    |
| `--color-danger`         | `oklch(55% 0.16 25)`   | エラー・削除・危険な操作               |
| `--color-warning`        | `oklch(55% 0.117 75)`  | 警告・注意喚起                         |
| `--color-success`        | `oklch(52% 0.14 148)`  | 成功・完了                             |
| `--color-text`           | `oklch(32% 0.025 255)` | 本文テキスト（青みのある濃いスレート） |
| `--color-text-secondary` | `oklch(50% 0.03 255)`  | 補足テキスト・ラベル・日付             |
| `--color-text-disabled`  | `oklch(64% 0.025 255)` | 無効状態のテキスト                     |
| `--color-border`         | `oklch(78% 0.023 255)` | 区切り線・枠・テーブル罫線             |
| `--color-background`     | `oklch(94% 0.006 255)` | ページ背景                             |
| `--color-surface`        | `oklch(92% 0.015 255)` | カード・コード・テーブルセル           |

### Typography

| 変数                | 既定値            | 用途                    |
| ------------------- | ----------------- | ----------------------- |
| `--text-huge`       | `1.6rem`（32px）  | H1                      |
| `--text-large`      | `1.2rem`（24px）  | H2                      |
| `--text-default`    | `1rem`（20px）    | H3 / 本文               |
| `--text-small`      | `0.75rem`（15px） | Caption・注釈・フッター |
| `--leading-default` | `1.6`             | 本文の行間              |
| `--leading-small`   | `1.2`             | 見出し（h1 / h2）の行間 |

### Spacing

block 方向は `rlh`（root line-height = 1.6 × 20px = **32px**）、inline 方向は `rem`（**20px**）が基準です。論理プロパティ（`margin-block` / `padding-inline` 等）と語彙を揃えており、縦書き（`writing-mode: vertical-rl` 等）でも軸が文書の流れに追従します。

| 変数                     | 既定値            | 用途                             |
| ------------------------ | ----------------- | -------------------------------- |
| `--space-block-tiny`     | `0.125rlh`（4px） | H3以上見出しの直後・リスト項目間 |
| `--space-block-small`    | `0.5rlh`（16px）  | コード・セル内パディング         |
| `--space-block-default`  | `1rlh`（32px）    | 見出し間・段落間                 |
| `--space-inline-default` | `1rem`（20px）    | 標準アキ                         |
| `--space-inline-large`   | `2rem`（40px）    | インデント・引用・リスト         |
| `--line-length`          | `40`              | 本文幅（`40ric` ≒ 800px @20px）  |

### Shape

| 変数       | 既定値                    | 用途                           |
| ---------- | ------------------------- | ------------------------------ |
| `--radius` | `var(--space-block-tiny)` | ボタン・入力欄・fieldsetの角丸 |

## License

MIT © 2026 [Kohei Watanabe](https://kou029w.github.io/)
