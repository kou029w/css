# @fogtype/css

**English** | [日本語](README.ja.md)

A minimal, classless CSS framework that conveys structure through typography and spacing alone.

Semantic HTML (`<h1>` / `<p>` / `<table>` / `<form>` …) turns into a polished layout as-is. Designed for CJK with mixed CJK and Latin text in mind, it uses `text-autospace` to automatically insert spacing between CJK and Latin characters, a margin rhythm based on `rlh` (root line-height), and a flat, shadow-free aesthetic.

## Overview

- **Readability** - The body width is constrained with `ric` (character-width based) to adapt to full width without breakpoints, with a flat, shadow-free aesthetic
- **Classless** - Plain HTML is styled as-is. Themes can be swapped simply by overriding the CSS variables on `:root` (single file, zero dependencies)
- **CJK support** - `text-autospace` adjusts spacing between CJK and Latin characters, and `text-spacing-trim` handles the spacing around punctuation automatically

To see it in action, view the [preview](https://kou029w.github.io/css/preview.html).

## Usage

### HTML (CDN)

Just add one line to your `<head>`.

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@fogtype/css" />
```

Then write semantic HTML.

```html
<!doctype html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@fogtype/css" />
  </head>
  <body>
    <h1>Heading</h1>
    <p>
      Body text is styled without any classes. Mixed 日本語 and English is
      auto-adjusted too.
    </p>
  </body>
</html>
```

### NPM

```sh
npm install @fogtype/css
```

To pull it in through a bundler:

```css
@import "@fogtype/css";
```

```js
import "@fogtype/css";
```

To use a local copy, just drop in [`index.css`](./index.css) as-is. It has no dependencies.

## Themes

Every theme is **declared as CSS variables (custom properties) on `:root`**. Color, typography, spacing, and corner radius all derive from this set of variables, so the entire theme can be swapped just by overriding them.

The variables fall into four groups.

| Group      | Prefix                        | Role                                                        |
| ---------- | ----------------------------- | ----------------------------------------------------------- |
| Color      | `--color-*`                   | All brand / semantic / neutral colors                       |
| Typography | `--text-*` / `--leading-*`    | Font sizes and line heights                                 |
| Spacing    | `--space-*` / `--line-length` | block (`rlh`) / inline (`rem`) margin rhythm and body width |
| Shape      | `--radius`                    | Corner radius of interactive elements                       |

For the default values of each variable, see [Legend - default theme (`:root`)](#legend-default-theme-root) below.

## Customizing

Override the variables on `:root` (or any scope) with your own CSS. The key is to place it **after** loading the framework itself (`index.css`).

<!-- prettier-ignore-start -->
```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@fogtype/css" />
<style>
  :root {
    --color-primary: oklch(54% 0.247 293);      /* swap the brand color */
    --color-primary-dark: oklch(40% 0.247 293); /* on hover */
    --radius: 0;                                /* sharpen the corners */
    --line-length: 50;                          /* widen the body to 50ric */
  }
</style>
```
<!-- prettier-ignore-end -->

To theme only part of a page, scope the variables to any element and re-declare them.

```css
.brand-section {
  --color-primary: oklch(52% 0.14 148);
  --color-background: oklch(96% 0.006 148);
}
```

## Legend - default theme (`:root`)

All tokens of the default theme declared on `:root` in `index.css`.

### Color

| Variable                 | Default                | Purpose                                        |
| ------------------------ | ---------------------- | ---------------------------------------------- |
| `--color-primary`        | `oklch(52% 0.165 255)` | Brand color, links                             |
| `--color-primary-dark`   | `oklch(44% 0.168 255)` | Primary on hover / press                       |
| `--color-primary-light`  | `oklch(88% 0.045 255)` | Selected rows, subtle highlights (`mark` etc.) |
| `--color-danger`         | `oklch(55% 0.16 25)`   | Errors, deletion, dangerous actions            |
| `--color-warning`        | `oklch(55% 0.117 75)`  | Warnings, cautions                             |
| `--color-success`        | `oklch(52% 0.14 148)`  | Success, completion                            |
| `--color-text`           | `oklch(32% 0.025 255)` | Body text (deep, bluish slate)                 |
| `--color-text-secondary` | `oklch(50% 0.03 255)`  | Supplementary text, labels, dates              |
| `--color-text-disabled`  | `oklch(64% 0.025 255)` | Text in the disabled state                     |
| `--color-border`         | `oklch(78% 0.023 255)` | Dividers, frames, table rules                  |
| `--color-background`     | `oklch(94% 0.006 255)` | Page background                                |
| `--color-surface`        | `oklch(92% 0.015 255)` | Cards, code, table cells                       |

### Typography

| Variable            | Default          | Purpose                           |
| ------------------- | ---------------- | --------------------------------- |
| `--text-huge`       | `1.6rem` (32px)  | H1                                |
| `--text-large`      | `1.2rem` (24px)  | H2                                |
| `--text-default`    | `1rem` (20px)    | H3 / body                         |
| `--text-small`      | `0.75rem` (15px) | Caption, notes, footer            |
| `--leading-default` | `1.6`            | Line height of body text          |
| `--leading-small`   | `1.2`            | Line height of headings (h1 / h2) |

### Spacing

The block axis is based on `rlh` (root line-height = 1.6 × 20px = **32px**) and the inline axis on `rem` (**20px**). The vocabulary is aligned with logical properties (`margin-block` / `padding-inline` etc.), so the axes follow the document flow even in vertical writing (`writing-mode: vertical-rl` etc.).

| Variable                 | Default          | Purpose                                      |
| ------------------------ | ---------------- | -------------------------------------------- |
| `--space-block-tiny`     | `0.125rlh` (4px) | Right after H3+ headings, between list items |
| `--space-block-small`    | `0.5rlh` (16px)  | Padding inside code / cells                  |
| `--space-block-default`  | `1rlh` (32px)    | Between headings, between paragraphs         |
| `--space-inline-default` | `1rem` (20px)    | Standard gap                                 |
| `--space-inline-large`   | `2rem` (40px)    | Indents, blockquotes, lists                  |
| `--line-length`          | `40`             | Body width (`40ric` ≈ 800px @20px)           |

### Shape

| Variable   | Default                   | Purpose                                     |
| ---------- | ------------------------- | ------------------------------------------- |
| `--radius` | `var(--space-block-tiny)` | Corner radius of buttons, inputs, fieldsets |

## License

MIT © 2026 [Kohei Watanabe](https://fogtype.com/@nebel)
