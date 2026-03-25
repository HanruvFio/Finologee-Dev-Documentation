# Finologee — Color-Coded Category Pills (CSS-Only Method)

## Overview

This method styles WordPress category tags as individually colored pills in Elementor carousels using **only native Elementor widgets and global CSS** — no custom PHP required.

Each category automatically gets its own color based on its URL slug:

- **BKO** → Blue pill
- **KYC** → Green pill
- **REG** → Orange pill
- **Everything else** → Red pill (default)

Works regardless of whether a post has 1 category or 6.

---

## Prerequisites

- Elementor Pro installed and activated
- A Loop Item template set up for your posts carousel
- Post categories properly assigned to your posts (e.g., BKO, KYC, REG)

---

## Step 1 — Configure the Post Info Widget

This outputs each category as its own separate link element instead of a concatenated string.

1. Open your **Loop Item** template in the Elementor Editor
2. Drag the **Post Info** widget into your design where you want the tags to appear
3. Under the **Content** tab, delete the default items (Author, Date, Time, Comments)
4. Add a new item and set its **Type** to **Terms**
5. Set the **Taxonomy** to **Categories**
6. Find the **Separator** field — delete the default comma (`,`) and replace it with a **single space**

> This is the key step. Replacing the comma with a space breaks the concatenated string into individual link elements that CSS can target independently.

---

## Step 2 — Assign the Custom CSS Class

This gives the widget a hook so the global CSS only targets these pills, not other lists on your site.

1. With the **Post Info** widget still selected, go to the **Advanced** tab
2. Open the **Layout** panel
3. In the **CSS Classes** field, type: `custom-category-pills`

> Do **not** include a dot (`.`) at the beginning — just the class name.

---

## Step 3 — Add the Global CSS

1. From any Elementor page or template editor, click the **Hamburger Menu** (top left) → **Site Settings**
2. Scroll down and click **Custom CSS**
3. Paste the following:

```css
/* ══════════════════════════════════════════════
   FINOLOGEE CATEGORY PILLS — CSS-Only Method
   ══════════════════════════════════════════════ */

/* 1. Flex container — align pills side-by-side */
.custom-category-pills .elementor-icon-list-items {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
}

/* 2. Base pill style — applies to ALL categories (Red/Default) */
.custom-category-pills .elementor-icon-list-item a {
    padding: 4px 12px;
    border-radius: 12px;
    font-size: 11px;
    font-weight: 600;
    text-transform: uppercase;
    text-decoration: none;
    color: #FF545D;
    background-color: #FF545D1C;
    transition: opacity 0.3s ease;
}

/* 3. Hover state for all pills */
.custom-category-pills .elementor-icon-list-item a:hover {
    opacity: 0.8;
}

/* 4. BKO — Blue */
.custom-category-pills .elementor-icon-list-item a[href*="/bko/"] {
    color: #218FAA;
    background-color: #218FAA1A;
}

/* 5. KYC — Green */
.custom-category-pills .elementor-icon-list-item a[href*="/kyc/"] {
    color: #6DA884;
    background-color: #6DA8841A;
}

/* 6. REG — Orange */
.custom-category-pills .elementor-icon-list-item a[href*="/reg/"] {
    color: #F78E25;
    background-color: #F78E2521;
}
```

---

## Step 4 — Verify & Troubleshoot

### Check Your Permalink Structure

The CSS targets the category slug inside each link's URL (e.g. `/bko/`). If colors aren't applying, hover over a category link on your live site and check the URL format, then adjust the selectors:

| Your URL looks like | Change selector to |
|---|---|
| `yoursite.com/category/bko/` (trailing slash) | `a[href*="/bko/"]` ← default, no change needed |
| `yoursite.com/category/bko` (no trailing slash) | `a[href*="/bko"]` |
| `yoursite.com/?category_name=bko` (plain permalinks) | `a[href*="=bko"]` |

### Reusing in Other Carousels or Templates

Whenever you want these colored pills elsewhere on the site:

1. Add a **Post Info** widget
2. Set it to **Categories**, remove the comma separator
3. Add `custom-category-pills` to the CSS Classes field

No need to touch the global CSS again — it applies everywhere the class is used.

### Adding Another Named Color

To give a new category (e.g. **AML**) its own color, just add one more CSS rule:

```css
/* AML — Purple */
.custom-category-pills .elementor-icon-list-item a[href*="/aml/"] {
    color: #BB6BD9;
    background-color: #BB6BD91A;
}
```

### Browser Compatibility Note

The 8-digit hex codes (e.g. `#218FAA1A`) include alpha transparency and are supported in all modern browsers. If you need to support very old browsers, replace them with `rgba()` equivalents:

```css
/* Example: #218FAA1A → rgba(33, 143, 170, 0.10) */
background-color: rgba(33, 143, 170, 0.10);
```

---

## Why This Method Works

The `a[href*="/bko/"]` CSS selector is an **attribute substring match** — it checks if the link's URL contains `/bko/` anywhere in it. Since WordPress category links always include the category slug in the URL, this automatically matches the right categories without any PHP logic. The base red style catches everything else as a fallback.
