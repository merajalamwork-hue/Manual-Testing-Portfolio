# 🛒 Saleor Storefront — Manual Testing

## About the Project

[Saleor](https://github.com/saleor/storefront) is an open source, headless eCommerce storefront built with Next.js and React, designed to work on top of the Saleor commerce API (GraphQL-based). It's a production-grade, actively maintained project used as a reference storefront implementation by developers building eCommerce experiences on the Saleor platform.

As part of my manual exploratory testing practice, I tested the storefront's responsiveness and core navigation across multiple device viewports (mobile, tablet landscape, and desktop) to evaluate UI consistency and look for functional or structural issues — particularly around the header navigation, since navigation is a critical path for any eCommerce flow.

---

## About the Bug

### 🟠 BUG-009 — Hamburger Menu Disappears on Tablet/Landscape Viewports + Hydration Error from Invalid HTML Structure

| Field | Details |
|---|---|
| Severity | Medium |
| Type | Responsive UI / DOM Structure (Hydration) Bug |
| Module | Header Navigation (Mobile Menu + Desktop Nav) |
| Status | ✅ Confirmed — Fix submitted via [PR #1212](https://github.com/saleor/storefront/pull/1212) |
| GitHub Issue | [#1198](https://github.com/saleor/storefront/issues/1198) |
| Date Reported | 2026 |
| Impact | Users on mid-size viewports lose all access to navigation; hydration mismatch produces React console errors affecting app reliability |

### What I Found

While testing the storefront's header across different screen sizes, I noticed that at certain viewport widths — particularly tablets in landscape orientation, like a Pixel 7 at `915px` — there was no visible navigation at all. Neither the mobile hamburger icon nor the desktop nav links were present.

Separately, while inspecting the page in dev tools, I noticed a React hydration warning being thrown on page load, which pointed to a mismatch between the server-rendered HTML and the client-rendered DOM.

### Steps to Reproduce

1. Open the Saleor storefront in a browser
2. Resize the viewport (or use device emulation) to a width between `768px` and `1024px` — for example, `915px` (Pixel 7 landscape)
3. Look at the header area where navigation should appear
4. Open the browser console and reload the page
5. Observe the console for hydration-related warnings/errors

### Expected Behavior

- At every viewport width, either the mobile hamburger menu or the desktop navigation links should be visible — there should be no gap where neither is shown
- The server-rendered markup and the client-rendered DOM should match exactly, producing no hydration warnings

### Actual Behavior

- **Missing navigation ("dead zone"):** The hamburger toggle was configured to hide at the `md` breakpoint (`768px`), but the desktop navigation didn't appear until the `lg` breakpoint (`1024px`). Any viewport between `768px` and `1024px` fell into a gap where neither menu rendered, leaving the site with no way to navigate.
- **Hydration error:** The desktop `<nav>` component rendered `<li>` elements directly, without wrapping them in a parent `<ul>`. The mobile menu's `<SearchBar>` component was also placed directly inside a `<ul>` without being wrapped in an `<li>`. Both are invalid HTML structures, and they caused the browser-parsed DOM to differ from the server-rendered output — triggering a React hydration error.

### Root Cause (Confirmed)

A contributor investigated the issue and confirmed both root causes directly:

- **Breakpoint mismatch:** mobile toggle used `md:hidden` while desktop nav used `lg:flex`, leaving viewports between the two breakpoints with no visible nav
- **Invalid HTML nesting:** missing semantic `<ul>`/`<li>` wrappers around nav and search elements caused the DOM mismatch responsible for the hydration error

### Fix

[PR #1212](https://github.com/saleor/storefront/pull/1212) resolves both issues by:
- Changing the hamburger menu visibility to `lg:hidden`, so it matches the desktop nav's `lg:flex` breakpoint and closes the dead zone
- Adding the required `<ul>`/`<li>` semantic wrappers around the desktop nav items and the mobile search bar, fixing the invalid HTML structure and eliminating the hydration mismatch

### Why This Matters

Navigation is a critical path on any eCommerce site — if a user can't open the menu, they can't browse categories, search, or access account/cart features. This bug would have silently broken that experience for anyone on a common tablet-landscape or similarly-sized viewport, without any error being visible to the end user (only in the dev console). It's a good example of how a small breakpoint inconsistency between two components can create a real, user-facing functional gap, and how invalid HTML structure can surface as a hydration error that's easy to overlook unless you're actively checking the console during testing.

---

## 👤 Author

**Meraj Alam**

GitHub: [@merajalamwork-hue](https://github.com/merajalamwork-hue)
