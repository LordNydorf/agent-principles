# UX Ruleset — Part 1: Pages, States, Forms, Buttons, Degradation

A drop-in ruleset for AI coding agents. Apply these rules to every page, form, and interactive component before considering the work complete. Do not ship code that violates any rule below without an explicit, stated reason.

---

## 1. Static vs. Dynamic Classification

Before writing any page, classify it:

- **Static**: no fetch, no submission, content does not change based on user or network (e.g. Terms of Service, Privacy Policy, About).
- **Dynamic**: fetches data, submits data, or reacts to user input — this includes forms, even simple ones.

**Rule:** Every dynamic page must explicitly implement all four states below (Loading, Error, Empty, Success) before it ships. A page that only implements the Success state is incomplete, regardless of how polished that state looks.

---

## 2. The Four States

Every dynamic page or component must handle:

- **Loading**
  - Use a skeleton screen for content-heavy views instead of a bare spinner, to prevent layout shift when data arrives.
  - Any loading state that can run long must have a timeout, a cancel option, or a retry option. Never leave a spinner with no way out.

- **Error**
  - Errors must be specific to what failed and where — not a single generic message for every possible failure.
  - Field-level errors appear inline at the field, not only in a top banner.
  - If multiple fields fail at once, provide a summary the user can jump to.

- **Empty**
  - Distinguish "nothing here yet" (new user, no data) from "nothing matched" (search/filter returned zero results). These require different messaging and different calls to action.

- **Success**
  - The result of an action must be perceivable on screen — a visible change, confirmation, or state update. Do not rely on an action "just working" with no visible confirmation.

**Rule:** None of the four states is optional. None may default to whatever is fastest to render.

---

## 3. Forms

- **Validation timing**: validate on blur (when a field loses focus), not on every keystroke. Do not show errors while the user is still actively typing in a field.
- **Focus management**: on failed submission, move focus to the first invalid field, or to an error summary that links to each problem field. Do not leave focus wherever it happened to be.
- **Autofill and password managers**: never block paste on any field. Expose correct `autocomplete` attributes so password managers and autofill work as expected, especially on login and signup forms.
- **Submit protection**: disable the submit button immediately on press, with a visible loading state, to prevent duplicate submissions from double-clicks or double-taps.

**Rule:** A form is not complete until validation timing, focus management, autofill compatibility, and submit-button protection are all implemented — not just the field layout and the happy-path submit call.

---

## 4. Buttons

- **Tap feedback**: every button must visibly respond within roughly 80–150ms of being pressed (opacity change, ripple, scale, or equivalent). Silence on press is a defect.
- **Disabled state**: must be semantically disabled (real `disabled` attribute/prop or platform equivalent), not merely styled to look inactive while still catching taps. Disabled must also be visually unmistakable, not just a slightly lighter shade.
- **Loading-within-button**: when a button shows a loading indicator in place of its label, the button must hold its width/size throughout, so the layout does not shift when the label is swapped out and back.
- **Touch target size**: minimum tappable area of 44x44pt (iOS) / 48x48dp (Android), even when the visible icon or label inside is smaller — pad the hit area out to the minimum rather than shrinking the requirement to match the icon.

**Rule:** A button is not complete until tap feedback, true disabled semantics, stable loading-state sizing, and minimum touch target size are all verified.

---

## 5. Graceful Degradation

- **Network drops mid-session**: detect and acknowledge connectivity loss (a persistent but unobtrusive indicator, clearly marked stale/cached content, or queued actions) rather than silently failing or showing stale data as if it were current.
- **Partial data**: if a response returns incomplete data, show what did load and explicitly mark what did not. Do not treat partial success as total failure (discarding usable data) or as full success (hiding the gap).
- **Feature unsupported**: if a feature is unavailable (missing browser API, unsupported plan, etc.), the corresponding control must be disabled or hidden with a short explanation — never left visible and tappable with no effect or a raw error.

**Rule:** Never let a degraded or partial state masquerade as full success or total failure. State what is missing, why, and what the user can still do.

---

## Pre-Ship Checklist

- [ ] Every page is classified as static or dynamic
- [ ] Every dynamic page implements Loading, Error, Empty, and Success
- [ ] Loading states have a timeout, cancel, or retry path
- [ ] Errors are field-specific and/or summarized, never generic-only
- [ ] Empty states distinguish "no data yet" from "no matches"
- [ ] Success states show a perceivable confirmation
- [ ] Forms validate on blur, not on keystroke
- [ ] Failed form submission moves focus to the first error or summary
- [ ] Forms never block paste; autocomplete attributes are correct
- [ ] Submit buttons disable immediately on press with a loading state
- [ ] All buttons show tap feedback within ~150ms
- [ ] Disabled buttons are semantically disabled, not just styled
- [ ] Loading-state buttons hold their width
- [ ] Touch targets meet 44x44pt / 48x48dp minimum
- [ ] Network loss is detected and acknowledged, not hidden
- [ ] Partial data is shown honestly, not collapsed into success or failure
- [ ] Unsupported features are disabled/hidden with an explanation, not left broken
