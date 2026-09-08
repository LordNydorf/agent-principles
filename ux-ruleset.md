# UX Ruleset — Agent Principles

A drop-in ruleset for AI coding agents. Apply these rules to every page, form, and interactive component before considering the work complete. Do not ship code that violates any rule below without an explicit, stated reason.

---

# Part 1: Pages, States, Forms, Buttons, Degradation

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

# Part 2: Foundational Psychology

## 6. Jakob's Law — Match Existing Conventions

Users spend most of their time on other apps, not yours. They arrive with expectations already formed — where the back button lives, what a hamburger icon means, how a swipe gesture behaves.

**Rule:** Default to the interaction pattern users already know for a given context (navigation, forms, gestures, icons) unless there's a specific, stated reason to deviate. Novelty in UI is a cost that must be paid for with a clear benefit — it is never free.

---

## 7. Hick's Law — Every Option Has a Tax

The time it takes someone to decide increases with the number and complexity of choices in front of them — for every user looking at that set of choices, not just the one who picks the option buried at the bottom.

**Rule:** Before adding another visible option to a screen, ask whether it needs to be visible by default or whether it can be grouped, defaulted, or tucked behind progressive disclosure. Every option added to the main view is a small tax charged to every user, not just the ones who need it.

---

## 8. Fitts's Law — Size and Distance Are Not Cosmetic

The time to move to and select a target depends on how far away it is and how big it is. Bigger, closer targets get hit faster and more accurately.

**Rule:** Primary actions belong close to where the user's hand already is — bottom-anchored on mobile, not top-anchored by default. Meet minimum touch target sizes (see Rule 4), and treat distance from the user's resting hand position as a first-class layout factor, not an afterthought.

---

## 9. Miller's Law — Chunk Past ~7 Items

People can hold roughly 7±2 discrete items in working memory at once. A flat list beyond that ceiling creates real cognitive strain, even when the user can't name why the screen feels exhausting.

**Rule:** Any screen presenting more than roughly 7 discrete items — fields, menu options, filters — should group them into clearly labeled chunks rather than presenting a single flat list.

---

## 10. Tesler's Law — Complexity Moves, It Doesn't Vanish

Every system has inherent complexity that cannot be removed, only moved between the system and the user. Someone has to deal with it — the only real choice is who.

**Rule:** Before removing a UI element in the name of simplicity, ask where its complexity goes. If the answer is "onto the user, every time they use this," that's not simplification — it's a transfer. Absorb complexity into the system when the system can handle it reliably once, rather than asking the user to handle it correctly every time.

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
- [ ] Interaction patterns (nav, gestures, icons) match existing conventions unless deviation is deliberate and justified
- [ ] No screen has more visible options than necessary; extras are grouped, defaulted, or hidden behind progressive disclosure
- [ ] Primary actions sit within easy reach of the user's resting hand position (bottom-anchored on mobile)
- [ ] Lists/forms with more than ~7 items are chunked into labeled groups
- [ ] Any simplification is checked for complexity it may have shifted onto the user rather than removed
