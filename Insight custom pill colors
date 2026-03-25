# Finologee — Color-Coded Category Pills for Elementor Loop Grid

## Overview

This guide gives you individually styled, color-coded category pills in your Elementor carousel/loop grid. Each category renders as its own pill with a color based on its slug:

- **BKO** → Blue pill
- **KYC** → Green pill
- **REG** → Orange pill
- **Everything else** → Red pill

Works regardless of whether a post has 1 category or 6.

---

## Step 1 — Add the PHP

Paste the following into your **child theme's `functions.php`** or into a **code snippets plugin** (WPCode, Code Snippets, etc.).

```php
<?php
/**
 * Finologee Category Pills Shortcode
 * Renders each post category as an individual color-coded pill.
 */

// ── Register the shortcode ──
add_shortcode( 'category_pills', 'finologee_category_pills_shortcode' );

function finologee_category_pills_shortcode( $atts ) {
    $atts = shortcode_atts( array(
        'post_id' => get_the_ID(),
    ), $atts );

    $categories = get_the_category( $atts['post_id'] );

    if ( empty( $categories ) ) {
        return '';
    }

    // Category slugs that get their own color. Everything else → 'default' (red).
    $named_colors = array( 'bko', 'kyc', 'reg' );

    $output = '<div class="fin-category-pills">';

    foreach ( $categories as $cat ) {
        $slug        = esc_attr( $cat->slug );
        $name        = esc_html( $cat->name );
        $link        = esc_url( get_category_link( $cat->term_id ) );
        $color_class = in_array( $slug, $named_colors, true ) ? $slug : 'default';

        $output .= sprintf(
            '<a href="%s" class="fin-pill fin-pill--%s">%s</a>',
            $link,
            $color_class,
            $name
        );
    }

    $output .= '</div>';

    return $output;
}
```

> **Note:** If you don't want the pills to be clickable links, change `<a href="%s"` to `<span` and the closing `</a>` to `</span>` in the `sprintf()` call.

---

## Step 2 — Add the CSS

Paste this into **Elementor → Site Settings → Custom CSS**, or into **Appearance → Customize → Additional CSS**, or into your child theme's `style.css`.

```css
/* ══════════════════════════════════════════════
   FINOLOGEE CATEGORY PILLS
   ══════════════════════════════════════════════ */

/* Container */
.fin-category-pills {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
    align-items: center;
}

/* Base pill */
.fin-pill {
    display: inline-block;
    padding: 4px 14px;
    border-radius: 50px;
    font-size: 12px;
    font-weight: 600;
    line-height: 1.4;
    text-decoration: none !important;
    letter-spacing: 0.02em;
    transition: opacity 0.2s ease;
    white-space: nowrap;
}

.fin-pill:hover {
    opacity: 0.85;
    text-decoration: none !important;
}

/* ── BKO — Blue ── */
.fin-pill--bko {
    background-color: rgba(33, 143, 170, 0.10);
    color: #218FAA;
}

/* ── KYC — Green ── */
.fin-pill--kyc {
    background-color: rgba(109, 168, 132, 0.10);
    color: #6DA884;
}

/* ── REG — Orange ── */
.fin-pill--reg {
    background-color: rgba(247, 142, 37, 0.13);
    color: #F78E25;
}

/* ── Default — Red (all other categories) ── */
.fin-pill--default {
    background-color: rgba(255, 84, 93, 0.11);
    color: #FF545D;
}
```

> **Note on colors:** Your background hex values (e.g. `#218FAA1A`) are 8-digit hex codes where the last two digits are the alpha/opacity. CSS support for 8-digit hex is inconsistent across older browsers, so the CSS above uses `rgba()` instead for full compatibility. The colors are identical.

---

## Step 3 — Update Your Loop Item Template

1. Open your **Loop Item template** in Elementor (the template used by your Loop Grid / carousel)
2. **Delete or hide** the existing widget that displays categories (the one currently showing the concatenated grey tags)
3. **Add a Shortcode widget** in its place
4. In the shortcode field, enter:

```
[category_pills]
```

5. **Save** the Loop Item template
6. Go back to the page with your carousel and refresh — each category should now appear as its own colored pill

---

## Step 4 — Fine-Tune

### Adjust Pill Size

```css
.fin-pill {
    padding: 4px 14px;      /* top-bottom | left-right */
    font-size: 12px;         /* text size */
    border-radius: 50px;     /* fully rounded ends */
}

.fin-category-pills {
    gap: 6px;                /* spacing between pills */
}
```

### Add Another Named Color

For example, to give **AML** a purple pill:

**1. PHP** — add `'aml'` to the array:

```php
$named_colors = array( 'bko', 'kyc', 'reg', 'aml' );
```

**2. CSS** — add the style:

```css
.fin-pill--aml {
    background-color: rgba(187, 107, 217, 0.10);
    color: #BB6BD9;
}
```

---

## Important: Check Your Category Slugs

This solution matches on the **category slug** (not the display name). Go to **Posts → Categories** in your WordPress admin and confirm:

| Display Name | Slug must be |
|---|---|
| BKO | `bko` |
| KYC | `kyc` |
| REG | `reg` |

If a slug differs (e.g. `kyc-compliance` instead of `kyc`), either:
- Edit the slug in WordPress to match, **or**
- Update the `$named_colors` array and CSS class to use the actual slug

---

## Troubleshooting

**Pills not showing at all**
- Make sure the shortcode is registered — check that the PHP code is active (snippet enabled, or `functions.php` saved without errors)
- Confirm the post actually has categories assigned

**All pills are red (default)**
- Your slugs don't match. Check Posts → Categories and compare slugs to the `$named_colors` array

**Pills still concatenated / old style showing**
- You likely still have the old taxonomy widget in the Loop Item template. Remove it and make sure only the Shortcode widget with `[category_pills]` remains

**Colors not applying**
- Check CSS specificity. If your theme overrides styles, add `!important` to the color properties, or wrap selectors more specifically, e.g. `.elementor-loop-container .fin-pill--bko { ... }`
