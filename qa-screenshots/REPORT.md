# Visual QA Report — systemco-site.vercel.app

**Date:** 2026-07-22
**Method:** Puppeteer (headless Chromium 150), viewport 1440×900, full-width slice screenshots every 800px + one full-page capture.
**Page height captured:** 7689 px (10 slices, `slice-00`…`slice-09`, plus `full-page.png`).

---

## Summary

The page is visually strong overall — consistent dark theme, coherent blue accent (`#0A84FF`), good typographic hierarchy on section headings, and a clean responsive grid. The issues below are ranked by severity. The **three critical** items were fixed in `index.html` this pass; the remainder are recommendations.

---

## 🔴 Critical (fixed this pass)

### C1. Hero subtitle & description are illegible over the background photo
- **Where:** `slice-00` (hero). Text "СИСТЕМКО — Москва" and "24/7 продвижение вашего продукта на большие рынки".
- **Problem:** `.hero-sub` used `--text-2` (`#9494B8`) and `.hero-desc` used `--text-3` (`#5C5C76`) — both very low-contrast greys — placed directly over the brightly-lit, high-detail server photo. The overlay gradient was near-transparent (0.35 alpha) exactly where the text column sits, so the copy nearly disappears against the lit server fans. Fails WCAG AA contrast.
- **Fix applied:**
  - Added a bottom-left radial **scrim** to `.hero-overlay` and darkened the tail of the linear gradient (0.72 → 0.9) so the text zone is anchored on a dark base.
  - Brightened `.hero-sub` to `#C7C7E0` (weight 500→600) and `.hero-desc` to `#A6A6C4`, and added a subtle `text-shadow` for legibility over any imagery.
- **Verified:** `verify-hero-after.png` — both lines now clearly readable.

### C2. Placeholder CTA label shipped to production
- **Where:** `slice-08`, "Техническая поддержка и поставка GPU" section.
- **Problem:** The primary button read **"Ссылка на новость"** ("link to news") — a developer placeholder, not a real label.
- **Fix applied:** Renamed to **"Смотреть новости"**, which matches the button's existing `href="#news"` target.

### C3. Stale copyright year
- **Where:** `slice-09` (footer).
- **Problem:** Footer read **"© 2024"** while the site's own content references 2026 events (ПМЮФ-2026, «Время цифр» 2026). Inconsistent and dates the site.
- **Fix applied:** Updated to **"© 2026"**.

---

## 🟡 Moderate (recommended — not auto-fixed; needs content/assets)

### M1. Visible content placeholders in cards
- **Where:** `slice-06`/`slice-07` ("Разработки наших партнёров") and `slice-01` (news cards).
- **Problem:** Case cards show literal **`[ Медиа ]`** placeholders and **"Логотип"** badges; news cards show empty dark thumbnails with only a play icon. These are intentional media slots (per the CSS comment "ready to receive gif/video files") but read as unfinished on a live site.
- **Recommendation:** Populate the media slots with real screenshots/GIFs and partner logos, or hide the `[ Медиа ]`/"Логотип" labels behind a gradient placeholder that doesn't read as debug text until assets land.

### M2. Excessive / uneven vertical whitespace between sections
- **Where:** hero→news, news→promo, "что мы делаем"→"как мы работаем" (`slice-01`, `slice-02`, `slice-03`).
- **Problem:** Several sections carry ~200–260px of empty dark space above their eyebrow label, more than the internal section rhythm. It reads as accidental gap rather than intentional breathing room.
- **Recommendation:** Normalize section top/bottom padding to a single scale (e.g. `clamp(5rem, 9vh, 8rem)`) so vertical rhythm is consistent.

## 🟢 Minor / polish

### P1. "Что мы делаем" heading vs. right-column description are baseline-misaligned
- `slice-03`: the big heading sits well below the right-aligned descriptive paragraph. Consider vertically centering the description against the heading block, or top-aligning both.

### P2. News card play-buttons imply video, but thumbnails are empty
- `slice-01`: five identical empty video tiles look repetitive. Poster frames would add visual interest and reduce the "template" feel.

---

## Screenshots index
| File | Region |
|------|--------|
| `full-page.png` | entire page (holistic) |
| `slice-00-y0.png` | Hero |
| `slice-01-y800.png` | News grid |
| `slice-02-y1600.png` | "Все новости" + Promo (продвижение) |
| `slice-03-y2400.png` | "Что мы делаем" directions |
| `slice-04-y3200.png` | Directions + "Как мы работаем" |
| `slice-05-y4000.png` | Partners marquee |
| `slice-06-y4800.png` | Partner cases (top) |
| `slice-07-y5600.png` | Partner cases (bottom) |
| `slice-08-y6400.png` | GPU / tech support + Contacts |
| `slice-09-y7200.png` | Contact form + Footer |
| `verify-hero-after.png` | Hero **after** the C1 fix (local render) |

*Note: fixes are in the local `index.html` and take effect on the next Vercel deploy.*
