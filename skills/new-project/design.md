# Design and UI/UX preferences

Use these preferences when planning or building a new interface. They describe
the user's general design preferences for web, mobile and desktop products.
Apply the interaction principles to terminal interfaces where relevant, using
terminal conventions for presentation.

Start with the user's task, then choose the smallest clear interface that supports
it. These are defaults, not a fixed visual template. New explicit instructions,
the product's established design system, accessibility needs and native platform
conventions can justify a different choice. Record the reason and affected scope.

## Foundation: Hick's Law

More options and harder-to-compare choices tend to increase decision time.

- When a quick response matters, reduce the choices users must evaluate at once.
- Give each task a clear primary action; make supporting actions quieter.
- Group related choices and distinguish them with familiar, descriptive labels.
- Break complex tasks into smaller, manageable steps that keep the next decision
  clear.
- Highlight a recommended option or sensible default when there is enough context
  to guide the choice.
- Introduce features progressively during onboarding. Help users complete a useful
  first action, then teach additional capabilities as they become relevant.
- Reveal advanced options progressively while keeping them discoverable.
- Preserve enough labels and context to make choices understandable. Removing
  useful information, replacing clear labels with ambiguous symbols or hiding
  common actions can make a task harder.

Do not impose an arbitrary maximum number of controls. Judge the complete task by
how easily users can find, understand and complete it, including any extra steps.

## Reusable preferences

### Keep setup focused and minimize manual input

- Ask for information needed to reach the first useful result. Leave optional
  policies, cosmetic customization and advanced tuning for later.
- Use resumable steps for setup with real dependencies. A single Continue action
  should validate, save and advance; avoid separate Save and Continue buttons.
- Enter the working product after verified completion. Avoid an additional welcome
  dialog or acknowledgement that adds no useful decision.
- Reuse known information and discover available choices where possible. Show
  readable names, offer search for long lists and preserve saved selections.
- Select an unambiguous sole option when appropriate. Ask when a choice changes
  access, cost or scope; discovery alone must not grant access.
- Generate internal identifiers and reuse saved information instead of asking
  people to copy technical values or enter the same information repeatedly.
  Keep a manual fallback where needed.
- Prefer reliable automatic values with clearly labeled optional overrides.

### Separate saved information from editing

- Show saved records as readable labels, values and statuses. Open focused New
  and Edit dialogs for ordinary management tasks; they can share an editor.
- Do not use an always-visible form or disabled inputs as the saved summary.
- Group closely related settings into one editor with one save action. Keep
  independent consequential actions separate.
- Use dedicated pages for substantial detail, history or workflows that need space
  or a direct link. Keep catalog cards short: identity, purpose and status.
- Keep sign-in, guided setup and workflows whose main purpose is editing on their
  natural surface. Use native sheets or full-screen editors when they fit better
  than a desktop-style modal.
- Show only configured/missing status for saved secrets; never echo their values.

### Make summaries compact and readable

- Lead with names, meaningful status and useful totals. Keep technical identifiers
  secondary, with an ID fallback when a name is unavailable.
- Use responsive grids of labels and values, wrapping tags and short grouped lists.
  Avoid oversized label columns, repeated separators and sparse stacks of rows.
- Put relevant status badges beside their heading. Place management actions at the
  top right of a card or section, within its padding; let long titles wrap.
- Keep dense tables focused. Move lengthy secondary details into a readable detail
  view, and offer an explicit way to reveal omitted items.
- Make summary counts lead to the records they count, using consistent definitions.
- Format dates, amounts, units and statuses for people. Operating the product should
  not require reading or editing raw JSON. Literal code, commands and user-authored
  content remain appropriate in their own editors or copyable blocks.

### Use a restrained, consistent visual language

- Prefer readable typography, clear grouping, subtle separators and restrained
  visual emphasis. Make important content and actions easy to distinguish.
- Choose colors, surfaces and control shapes to suit the product's identity and
  context. Preserve contrast and consistent meanings across supported themes.
- Keep branding simple and recognizable. Avoid decorative effects or motion that
  distract from the task; use animation for meaningful state feedback.
- Reuse shared components and semantic tokens for cards, fields, menus, dialogs,
  notifications and pagination. Comparable controls should look and behave alike.
- Keep selectors consistent and make their current value, available interactions
  and selected state clear. Preserve search, keyboard behavior, validation and
  disabled states; use native controls where fitting.
- Use one coherent icon set suited to the product and platform.

### Keep actions clear and visually ordered

- Prefer short Edit and New entry labels when context identifies the target. Pair
  a labeled New button with a plus icon. Accessible names can identify the object.
- Keep visible, specific text for submission, sign-in, authorization, approval and
  other actions whose consequences need explanation.
- Reserve primary styling for the task's main submit action. Supporting connection,
  recheck and recovery actions use secondary styling.
- Use icons alone only for familiar operations in clear context. Prefer borderless
  icon buttons with hover feedback, visible focus and explicit accessible names.
- Use a simple outline bin and a danger color for deletion. Explain consequential
  removal or interruption before confirmation, including what will remain.
- Keep routine actions prominent and recovery or advanced operations in
  a separate, discoverable area. Do not make uncommon actions compete equally.
- Put switches beside their labels. Make it clear whether a switch saves
  immediately or belongs to a draft requiring submission.

### Make dialogs predictable

- Give dialogs a descriptive heading and a plain X close icon without an enclosing
  circle. Keep an accessible name and a comfortable hit area.
- Default to the primary action followed immediately by Cancel in the same row.
  Preserve that reading and keyboard order when wrapping; follow native platform
  ordering when the platform supplies the dialog.
- Cancel discards the draft. Failed saves keep the dialog and entered values open,
  with validation or request errors visible inside it.
- Prevent duplicate submission. During a pending save that cannot safely be
  interrupted, block dismissal and show progress; restore controls on failure.
- Support named dialogs, contained keyboard focus, Escape when dismissible, focus
  return and scrolling that keeps the form usable on small screens.

### Preserve context while loading and updating

- Render known headings, labels, navigation and sections immediately. Skeletons
  belong only where data is still unknown; reserve their space to limit movement.
- Keep loaded content, selections, expanded details and drafts visible during
  background refresh. Disable only actions that depend on unavailable data.
- Distinguish unknown, loading, empty, disconnected and measured zero states.
- Keep submitted forms and entered values visible while showing button progress.
  Navigate only when the destination is ready; failures should allow retry.
- Before session or initial routing state is known, use a neutral startup surface
  rather than briefly showing an incorrect sign-in or setup screen.
- Load on entry, update after mutations and refresh automatically where useful.
  Avoid routine refresh buttons in every heading. Offer recovery after failure;
  explicit refresh is appropriate for logs, diagnostics and deliberate update checks.
- Background updates must preserve drafts and the user's reading position.

### Put feedback where users can act on it

- Use compact, non-blocking notifications for routine confirmations without
  shifting content. Keep styling quiet and status recognizable.
- Give transient feedback enough time to read, pausing dismissal while hovered or
  focused. Keep actionable errors visible until dismissed or resolved, near the
  affected task where possible. Offer a clear recovery action and announce
  feedback accessibly.
- Keep field validation beside its field, dialog errors inside the dialog and
  durable readiness warnings or setup guidance beside the relevant controls.
- Use a brief check-state change for a successful copy rather than another toast.
- Explain failures in readable language with a concrete recovery step and who can
  take it. Preserve entered values; avoid exposing raw logs or sensitive details.
- For longer work, distinguish working, waiting, stopping, complete and failed.
  Update an existing progress surface instead of accumulating repetitive notices.
  Report confirmed outcomes; an accepted stop request is not confirmed termination.

### Make compactness work across devices and themes

- Preserve readable content and comfortable controls as layouts narrow. Let grids,
  tags and action rows wrap; contain wide-table scrolling within the data region.
- For a sidebar-based product, prefer a dismissible mobile navigation sheet over
  stacking the entire sidebar above the page. Native app navigation may differ.
- Keep touch targets around 44px where practical even when glyphs are small.
  Provide accessible names, visible keyboard focus and status cues beyond color.
- Support light and dark themes with shared tokens. Default to system preference
  where available, remember explicit choices and avoid a flash of the wrong theme.
- Respect safe areas, reduced motion and platform navigation conventions. Prefer
  native host capabilities when they fit, without duplicating back/close controls.
- Let the content container own spacing between grouped items. When removing,
  hiding or moving content, remove obsolete margins and empty wrappers; inspect
  the first and last visible items for balanced padding.

## Applying and maintaining the preferences

Before implementation, identify the applicable rules and turn them into observable
acceptance criteria. Reuse a comparable component after checking it against those
criteria; existing UI may contain older inconsistencies.

Verify the rendered result at wide and narrow widths and in supported themes.
Check saved display separately from editing, plus relevant loading, empty, pending,
error and long-content states. Check keyboard/focus behavior and affected container
spacing. Passing builds or save tests alone does not verify the design. State any
unverified behavior explicitly; add focused regression coverage for changed
interaction contracts when appropriate.

For a new UI project, adapt these defaults into `docs/design/ui-rules.md` and link it
from the project's `AGENTS.md`. Keep project decisions there with their scope,
exceptions and source. A clear new user request can establish a preference within
its scope; update the affected rule when it changes. Keep examples distinct from
standing preferences, and do not generalize a local exception without evidence.
