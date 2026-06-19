# 🧪 Functional Testing — Plane.so
> Bugs discovered during manual exploratory testing of Plane.so — an open source project management SaaS application.

🔗 **Application:** [Plane.so](https://app.plane.so)
🔗 **Source Repo:** [github.com/makeplane/plane](https://github.com/makeplane/plane)
🔗 **Testing Type:** UI & Responsive / Cross-platform / Functional / Input Validation Testing

---

## 📊 Summary

| Metric | Count |
|--------|-------|
| Bugs Found | 2 |
| UI / Responsive Bugs | 1 |
| Input Validation Bugs | 1 |
| Severity | Medium / Low |
| Reported to Plane GitHub | ✅ Yes (both) |

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

### ✨ ENHANCEMENT-001 — Popup Menu Border/Shadow Visibility for Better UI Separation

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

---

## BUG-002 — Workspace Name Field Accepts Symbol-Only Input Despite Active Character Validation

| Field | Details |
|-------|---------|
| ID | BUG-002 |
| Severity | Low |
| Type | Input Validation Bug |
| Module | Workspace Settings |
| Component | General Settings → Workspace Name Field |
| Status | Open — Assigned to @pushya22, @vihar |
| Reported To | [makeplane/plane#9255](https://github.com/makeplane/plane/issues/9255) |
| Date | 2026-06-17 |
| Response Time | Triaged, labeled, and assigned within 9 minutes of filing |

### Description
The Workspace name field in Workspace Settings → General validates character composition in real time — entering a disallowed character (e.g. a comma `,`) is immediately rejected with the inline error: *"Workspace name can only contain letters, numbers, spaces, hyphens, and underscores."*

However, this validation only checks **character type**, not **character content**. A workspace name made up entirely of hyphens, underscores, and/or spaces — with zero letters or numbers — passes validation and is saved successfully, producing a non-descriptive workspace name (e.g. `-_________-`) that persists across the UI: settings header, sidebar, and workspace switcher.

### Steps to Reproduce
1. Go to **Workspace Settings → General**
2. In the **Workspace name** field, clear the existing name and enter: `-_________-`
3. Click **Update workspace**
4. Refresh the page

### Expected Result
Given the field already enforces a character allowlist with real-time feedback, it should also reject names with zero alphanumeric characters — e.g. with an error like *"Workspace name must contain at least one letter or number."*

### Actual Result
- Toast confirms: **"Workspace updated successfully"**
- The name `-_________-` persists after page refresh
- The symbol-only name renders across sidebar, settings header, and workspace switcher

### Evidence Table

| Input | Contains Alphanumeric? | Result |
|-------|----------------------|--------|
| `dl,I` (comma — disallowed char) | Yes | ❌ Rejected — inline error shown |
| `-_________-` (all allowed chars, zero alphanumeric) | No | ✅ Accepted and persisted |

### Environment

| Field | Details |
|-------|---------|
| Platform | Web — app.plane.so |
| Browser | Safari (macOS) |
| Role | Workspace Owner |
| Variant | Cloud |
| Environment | Production |
| Version | Latest |

### Testing Techniques Applied
- Exploratory Testing
- Boundary / Input Validation Testing
- Character allowlist edge case analysis
