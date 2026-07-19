---
name: bsd-branding
description: Apply the BSD XR (Bit Space Development Ltd.) brand identity — colour tokens, Space Mono typography, logo rules, and flat/sharp-edged UI component patterns. Use whenever building, styling, or reviewing any UI, app, deck, or generated content for BSD XR / bsdxr.com, or whenever a project references "BSD XR branding/theme". Bundles theme.json (machine-readable tokens), theme.schema.json (JSON Schema for validation), and the logo/favicon files needed to apply the brand without leaving this folder.
---

# BSD XR Brand Skill

This is a **self-contained, portable** skill. Everything needed to apply BSD XR
branding lives in this folder — copy the whole `bsd-branding/` directory into
any project's `.claude/skills/` to give that project's agents instant, offline
access to the brand system. Nothing here depends on files outside this folder.

Canonical source of truth (for humans, and to sync updates back to): the
[bsd-branding-repo](https://github.com/BitSpaceDevelopment/bsd-branding-repo)
— live guide at bitspacedevelopment.github.io/bsd-branding-repo.

## Files in this skill

| File | Use it for |
|---|---|
| `theme.json` | Machine-readable colour + typography tokens. Read this first for exact hex/RGB values — don't guess or approximate colours. |
| `theme.schema.json` | JSON Schema for `theme.json`. Validate any edits against it before treating a change as done. |
| `logos/logo-dark.png` | White wordmark + gradient XR mark. Use on dark backgrounds only. |
| `logos/logo-light.png` | Black wordmark + gradient XR mark. Use on light backgrounds only. |
| `icons/favicon.png` | Browser favicon. |

The full icon set (iOS `Assets.xcassets`, Android mipmaps, App Store / Play
Store assets) is intentionally not bundled here to keep this skill small.
Fetch it from the branding repo's `icons/` directory or the
[`AppIcons.zip` release](https://github.com/BitSpaceDevelopment/bsd-branding-repo/releases/latest/download/AppIcons.zip)
only if the task specifically needs a native app icon set.

## Non-negotiable design rules

1. `border-radius: 0` everywhere — no rounded corners.
2. `border-width: 1px` — never thicker.
3. No `box-shadow` / `drop-shadow`, anywhere.
4. No gradients in UI elements. The XR gradient (see `logos.brandBlue` in
   `theme.json`) is reserved exclusively for the logo mark.
5. Font is **Space Mono** at all sizes — body, labels, code, inputs. No other font.
6. All user-facing UI text is **uppercase**, with wide letter-spacing
   (`0.12em`–`0.25em`; see `typography.letterSpacing`).
7. Accent colour (`theme.themes.<mode>.colors.accent`) is used only on
   interactive elements — buttons, links, active states. Never decorative.
8. Sparse layouts — generous whitespace, content never fights for attention.

## Colour tokens

Read exact values from `theme.json` → `themes.dark.colors` / `themes.light.colors`.
Do not hardcode hex values in generated code from memory — always read them from
the file so token updates stay in sync.

Values are exposed as CSS custom properties with space-separated RGB channels
(Tailwind opacity-modifier compatible):

```css
:root {
  --color-background:   10 10 10;
  --color-accent:       255 255 255;
  /* full set in theme.json → themes.dark.colors */
}
:root.light {
  --color-background:   238 238 238;
  --color-accent:       27 79 216;
  /* full set in theme.json → themes.light.colors */
}
```

```html
<div style="background: rgb(var(--color-surface)); color: rgb(var(--color-text) / 1)">
```

Theme is toggled via a `light` class on `<html>`, persisted to
`localStorage['bsdxr-theme']`. See `theme.json` → `implementation` for the full
contract.

Status colours (`theme.json` → `themes.<mode>.statusColors`) are fixed and do
not change between dark/light themes.

## Logo rules

- Never place `logo-dark.png` on a light background, or `logo-light.png` on a
  dark background.
- Never flatten the XR gradient to a single colour, stretch, rotate, skew, or
  apply filters/effects to either logo.
- Maintain clear space equal to the cap-height of "B" on all sides.

## Typography

- Font: `Space Mono` (weights 400, 700). Import URL is in
  `theme.json` → `typography.googleFontsUrl`.
- All UI text uppercase, wide tracking. No sentence case in labels, buttons,
  headings, or nav items.

## Component patterns (Tailwind shorthand)

```
Button primary: border border-accent text-accent px-4 py-1.5 uppercase tracking-widest text-xs
                hover:bg-accent hover:text-background
Button ghost:   border border-border text-muted px-4 py-1.5 uppercase tracking-widest text-xs
                hover:border-border-light hover:text-text
Input:          bg-surface border border-border text-text font-mono text-sm px-3 py-2
                outline-none focus:border-border-light
Badge:          border px-1.5 py-0.5 text-[10px] uppercase tracking-widest
```

## Voice & tone (for generated copy)

Direct, no filler. Active voice, short declarative sentences. No superlatives,
buzzwords, or exclamation marks. Technical but accessible — speak to engineers
and executives in the same breath.

## Keeping this skill in sync

If brand tokens change upstream, re-copy `theme.json`, `theme.schema.json`, and
the two logo PNGs from the
[bsd-branding-repo](https://github.com/BitSpaceDevelopment/bsd-branding-repo)
root into this folder — this skill intentionally duplicates those files rather
than referencing the parent repo, so it stays fully portable on its own.
