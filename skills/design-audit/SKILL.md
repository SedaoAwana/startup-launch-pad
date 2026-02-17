# Design Audit & Standardization

A systematic methodology for auditing, standardizing, and fixing design inconsistencies across frontend codebases. Distilled from real-world full-stack design overhauls — button colors, backgrounds, dark mode, WCAG compliance, and component system alignment.

Use this skill when a codebase has inconsistent colors, mismatched buttons, broken dark mode, accessibility issues, or needs a design system standardization pass.

---

## Methodology: 4 Phases

### Phase 1: Discovery & Audit

Before changing a single line, understand the full picture. Run all discovery steps in parallel.

**1A. Brand & Design System Inventory**

Find and read:

- Brand guide / design docs (check `/docs/`, `BRAND_GUIDE.md`, `STYLE_GUIDE.md`)
- Tailwind config (`tailwind.config.*`) — custom colors, theme extensions
- Global CSS (`globals.css`, `index.css`) — CSS variables, custom classes, dark mode overrides
- Reusable components (`Button.tsx`, `FormButton.tsx`, any `/ui/` or `/brand/` directories)
- Component libraries (shadcn, radix, custom design system)

Report:

- Official brand colors and their intended usage
- CSS variables defined (light and dark mode)
- Reusable components that exist vs. what's actually used
- Gap between documented design system and actual implementation

**1B. Color Census**

Search the ENTIRE frontend for every button color pattern:

```
bg-blue-*, bg-purple-*, bg-green-*, bg-red-*, bg-gray-*, bg-teal-*,
bg-indigo-*, bg-emerald-*, bg-orange-*, bg-yellow-*, bg-cyan-*, bg-sky-*
```

For each color found on interactive elements, report:

- Exact Tailwind classes used
- Number of instances
- Which files
- Whether it's a primary CTA, secondary action, or destructive action
- Whether it has proper hover, focus, and disabled states
- Whether it has dark mode handling

Also search for:

- CSS class-based buttons (`btn-*`, `form-btn-*`, component variants)
- Inline style buttons
- Buttons using CSS variables

**1C. Background & Surface Hierarchy**

Document the current layering:

- Body/HTML background color
- Page container backgrounds
- Card/panel backgrounds
- Input/form backgrounds
- Modal/overlay backgrounds

Check: Is it flat (everything same color) or layered (background < surface < elevated)?

**1D. Dark Mode Audit**

Check implementation:

- How is dark mode triggered? (CSS class, media query, data attribute)
- Does it detect OS preference (`prefers-color-scheme`)?
- Are there pages/components missing dark mode styles entirely?
- Are there conflicting systems? (CSS `!important` overrides vs Tailwind `dark:` prefix vs CSS variables)
- Do CSS overrides become stale after color migrations?

List every page with incomplete dark mode support.

**1E. WCAG Accessibility Check**

For every button color found, calculate contrast ratios:

| Background | Text | Ratio | WCAG AA (4.5:1) | WCAG AAA (7:1) |
| ---------- | ---- | ----- | --------------- | -------------- |

Common failures:

- Light/medium colors (sky-400, teal-400, blue-400) with white text — usually fail
- Need darker shades (600-800 range) for white text to pass AA
- Alternative: use dark text on light/medium backgrounds

Flag any button that fails WCAG AA.

---

### Phase 2: Define the Standard

Before executing changes, establish the exact target state. Get user approval.

**2A. Button Color System**

Define exactly 3-4 button tiers:

| Tier            | Usage                       | Example Classes                           |
| --------------- | --------------------------- | ----------------------------------------- |
| **Primary CTA** | Save, Submit, Create, Apply | `bg-[color] hover:bg-[darker] text-white` |
| **Secondary**   | Cancel, Back, filters       | Outlined or neutral gray                  |
| **Destructive** | Delete, Remove, Reject      | `bg-red-600 hover:bg-red-700 text-white`  |
| **Ghost/Text**  | Minor actions, links        | `text-[color] hover:underline`            |

Verify the primary color passes WCAG AA (4.5:1 minimum with text color).

**2B. Background System**

Define the surface hierarchy:

| Layer    | Light Mode      | Dark Mode        | Purpose                   |
| -------- | --------------- | ---------------- | ------------------------- |
| Body     | off-white       | dark gray        | Base, reduces eye strain  |
| Surface  | slightly tinted | slightly lighter | Content areas             |
| Elevated | white           | lighter still    | Cards float with contrast |

Never use pure `#FFFFFF` as body background for apps used all day. Use `#F8FAFC` (slate-50), `#FAFAFA`, or similar.

**2C. Element Preservation List**

Explicitly define what NOT to change:

- Toggle switches / checkbox states
- Tab indicator bars
- Chart bars / data visualization colors
- Status badges and count badges
- Avatar background circles
- Progress bars
- Chat message bubbles
- Unread notification dots
- Timeline dots

These are semantic color uses — they communicate meaning, not brand.

---

### Phase 3: Execute Changes

**3A. CSS Variables & Theme (Do First)**

Update global CSS variables. This is typically 2-5 lines:

- Light mode background/surface values
- Verify dark mode values are consistent
- Update any CSS-based button classes (btn-solid-primary, etc.)
- Fix theme context/provider if OS preference detection is missing

**3B. Button Replacement (Run in Parallel)**

Split into 5+ parallel batches by directory:

- Batch 1: Main app pages (e.g., `/app/agency/**`)
- Batch 2: Secondary app pages (e.g., `/app/client/**`)
- Batch 3: Portal pages (e.g., `/app/candidate/**`)
- Batch 4: Shared components (`/components/**`)
- Batch 5: Public pages (`/app/[public]/**`)

Each batch agent gets identical rules:

```
Replacement rules (BUTTONS ONLY):
- bg-[old-color] -> bg-[new-color]
- hover:bg-[old-hover] -> hover:bg-[new-hover]
- dark:bg-[old-dark] -> remove if new color works in both modes
- disabled:bg-[old]-400 -> disabled:opacity-50
- focus:ring-[old]-500 -> focus:ring-[new]-500

DO NOT CHANGE:
- Non-button elements (badges, toggles, tabs, progress bars, avatars)
- Outlined/ghost/secondary buttons
- Destructive (red) buttons
```

**3C. Dark Mode Fixes**

For pages with missing dark mode:

- Only add `dark:` Tailwind prefix classes
- Use standard values: `dark:bg-gray-900`, `dark:bg-slate-800`, `dark:text-white`, `dark:border-gray-700`
- Do NOT create new components or abstractions
- Do NOT restructure files

**3D. Component System Alignment**

After inline buttons are fixed, check:

- CSS class definitions (`btn-solid-primary`, `form-btn-*`) — do they reference old colors?
- Reusable Button components — do their variant mappings use old colors?
- FormButton / portal-specific buttons — are they consistent?

These are easy to miss because they're one level of indirection away from the templates.

---

### Phase 4: Verify

**4A. Comprehensive Audit (Read-Only)**

Run a verification agent that checks:

1. CSS variables are correct (light and dark mode)
2. Spot check 10 files across different areas for correct button pattern
3. Search for ANY remaining old-color button patterns (should be zero)
4. Verify non-button elements were preserved (toggles, badges, etc.)
5. Check for stale CSS `!important` overrides that now affect wrong elements
6. Over-engineering check: zero new components, utilities, or abstractions created
7. Scan for obvious TypeScript/syntax errors

Report PASS / FAIL / WARNING per section.

**4B. Straggler Search**

This is critical — search for ALL color variants that could have been missed:

```
Search for BOTH:
1. The color you changed FROM (verify zero buttons remain)
2. Any ORIGINAL colors that were never part of the conversion chain
```

Example: If you changed blue -> gray -> teal, also search for purple buttons that were always purple and never went through the conversion chain. These are the most common miss.

**4C. CSS Override Cleanup**

After migration, check for `!important` overrides in global CSS that targeted the OLD color:

```css
/* Example: This was for inverting gray-900 buttons in dark mode */
/* After migration to teal, it now incorrectly inverts tooltips */
html.dark .bg-gray-900 {
  background-color: #f1f5f9 !important; /* REMOVE — stale override */
}
```

Remove or re-scope any overrides that are now over-broad.

---

## Common Pitfalls

### 1. Missing the Second Color

You search for `bg-blue-600` buttons, change them all. But there were also `bg-purple-600` buttons in settings pages that were ALWAYS purple. The initial search never found them because you only searched for blue.

**Fix:** After the main conversion, search for ALL colored backgrounds on buttons, not just the one you started with.

### 2. CSS Component Mismatch

Inline buttons are teal, but the `Button.tsx` component's `primary` variant still maps to purple via a CSS class.

**Fix:** Always check CSS class definitions AND component variant mappings after changing inline styles.

### 3. Stale Dark Mode Overrides

A CSS rule like `.dark .bg-gray-900 { background: white !important }` was designed to invert buttons. After buttons move to teal, this rule now inverts tooltips and other intentionally-dark elements.

**Fix:** Review all `!important` overrides after migration. Remove ones that no longer serve their purpose.

### 4. WCAG Failure

Picking a color that looks good but fails accessibility. Medium-light colors (400-500 range) almost always fail with white text.

**Fix:** Calculate contrast ratios BEFORE choosing. Use 600-800 range shades for white text. Or use dark text on lighter backgrounds.

### 5. Over-Engineering

Creating a new `PrimaryButton` wrapper component, adding a color config file, building a theme system — when all you needed was to find-and-replace Tailwind classes.

**Fix:** The right solution is almost always surgical class swaps. No new abstractions. No new files.

---

## Checklist Before QA

- [ ] Zero old-color button patterns remaining
- [ ] Zero stale CSS overrides
- [ ] All button component variants aligned with new colors
- [ ] Background hierarchy creates visible card elevation
- [ ] Dark mode detects OS preference on first visit
- [ ] Dark mode works on all pages (no broken/missing pages)
- [ ] Primary button color passes WCAG AA (4.5:1+)
- [ ] Non-button elements (toggles, badges, avatars, progress bars) preserved
- [ ] No new components, utilities, or abstractions were created
- [ ] Build compiles without errors
