# 🧪 Functional Testing — Plane.so

> Bugs discovered during manual exploratory testing of Plane.so — an open source project management SaaS application.

🔗 **Application:** [Plane.so](https://app.plane.so)
🔗 **Source Repo:** [github.com/makeplane/plane](https://github.com/makeplane/plane)
🔗 **Testing Type:** UI & Responsive / Cross-platform Testing

---

## 📊 Summary

| Metric | Count |
|--------|-------|
| Bugs Found | 1 |
| UI / Responsive Bugs | 1 |
| Severity | Medium |
| Reported to Plane GitHub | ✅ Yes |

---

## BUG-001 — "+ Add Quick Link" Text Overlaps Plane AI Promotional Text on Mobile Browser

| Field | Details |
|-------|---------|
| ID | BUG-001 |
| Severity | Medium |
| Type | UI / Responsive Layout Bug |
| Module | Home Page |
| Component | Quicklinks Section / Plane AI Section |
| Status | Open |
| Reported To | [makeplane/plane#9084](https://github.com/makeplane/plane/issues/9084) |
| Date | 2026-05-15 |

### Description
On the Plane.so home page, when accessed via mobile Chrome browser, the **"+ Add quick Link"** button text from the Quicklinks section visually overlaps with the **Plane AI promotional text** ("Use Build mode to create work items, Cycles and more."). This creates visual confusion as two separate UI elements render on top of each other.

The issue is **specific to mobile browser layout** and does not occur in desktop mode.

### Steps to Reproduce
1. Open Chrome browser on a mobile device
2. Navigate to https://app.plane.so
3. Sign in with your account
4. Observe the Home page immediately after login
5. Look at the area between the **Plane AI** section and the **Quicklinks** section

### Expected Result
The "+ Add quick Link" button and the Plane AI promotional text should be clearly separated with proper spacing, rendering in their own distinct sections without any visual overlap.

### Actual Result
The "+ Add quick Link" text overlaps with the Plane AI description text, causing two UI elements from different sections to visually merge on mobile viewport.

### Desktop vs Mobile Comparison

| Mode | Behaviour |
|------|-----------|
| Desktop browser | ✅ No overlap — layout renders correctly |
| Mobile Chrome browser | ❌ Overlap occurs — sections bleed into each other |

### Environment
| Field | Details |
|-------|---------|
| Device | Samsung Galaxy A22 4G |
| OS | Android 13 |
| Browser | Chrome 148.0.7778.120 |
| Variant | Cloud |
| Environment | Production |
| URL | https://app.plane.so |

### Evidence
`plane_mobile_ui_overlap.jpeg` — screenshot showing overlap on mobile Chrome immediately after login

👉 **Reported Issue:** [makeplane/plane#9084](https://github.com/makeplane/plane/issues/9084)

### ✨ ENHANCEMENT-001 — Popup Menu Border/Shadow 
Visibility for Better UI Separation

| Field | Details |
|-------|---------|
| Type | UX / Accessibility Enhancement |
| WCAG | 1.4.11 Non-text Contrast |
| Platform | Web — app.plane.so |
| Status | 🔄 In Progress — Being implemented by @aggmoulik |

🔗 [View Issue #9095](https://github.com/makeplane/plane/issues/9095)

### Testing Techniques Applied
- Responsive / Cross-platform Testing
- Exploratory Testing
- Desktop vs Mobile comparison testing
