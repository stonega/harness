---
name: live-system-chrome
description: Verify local web applications in the user's already-running system Chrome and real browser profile through agent-browser auto-connect, without launching a separate Chrome. Use whenever the user asks to check a page they opened, reuse their current Chrome/profile/login state, attach to an existing browser, or avoid a fresh automation browser.
metadata:
  compatibility: Requires agent-browser with auto-connect support and Google Chrome 144 or newer.
---

# Live System Chrome Verification

Use the user's already-running Chrome window as the source of truth for rendered UI, authentication, browser state, and screenshots. Keep the workflow read-only unless the user explicitly asks for interaction.

## Prerequisite

Chrome 144+ can accept a live debugging connection from the current profile:

1. Ask the user to open `chrome://inspect/#remote-debugging` in the Chrome profile containing the target page.
2. Ask them to enable **Allow remote debugging for this browser instance**.
3. Ask them to open or focus the target page and say `ready`.
4. When Chrome displays an incoming debugging prompt, ask the user to click **Allow**.

If the user already said `ready`, attempt the connection immediately. Only repeat the setup instructions if attachment fails.

Do not launch Chrome with `--remote-debugging-port`, use `--profile Default`, or start a headed agent-browser session for this workflow. Those approaches create or copy a separate browser environment instead of controlling the window the user opened.

## Attach and identify the target

List the live tabs:

```bash
agent-browser --auto-connect tab list
```

Identify the target from the active marker, title, or URL. Treat every unrelated tab as private: do not repeat unrelated titles, URLs, account names, or message counts in commentary or the final report.

Tab IDs such as `t2` belong to the current connection. If an ID is rejected, list tabs again and use the new exact ID.

## Verify in one batch

Run dependent checks in one process after resolving the target tab. Replace `t2` and the screenshot path with explicit values:

```bash
agent-browser --auto-connect batch --bail \
  "tab list" \
  "tab t2" \
  "get url" \
  "get title" \
  "snapshot -i" \
  "errors" \
  "screenshot /tmp/live-system-chrome-result.png"
```

Use command-string arguments as shown. In managed environments, piping JSON into `batch` can lose access to the agent-browser runtime socket, while separate commands can lose the live attachment and fall back to `about:blank`.

Pass `--auto-connect` on every new invocation. Avoid a named `--session` for the live system browser unless it has already been proven to retain that connection.

After capturing the screenshot, inspect it with the available local-image viewing tool. An accessibility snapshot alone does not verify spacing, clipping, charts, canvas content, colors, or responsive layout.

## Focused diagnostics

The default check consists of:

- target URL and title
- interactive accessibility snapshot
- page-error collection
- viewport screenshot and visual inspection

Only collect the full console when it helps answer the request. Trading and streaming applications can accumulate thousands of routine messages, which obscures relevant evidence and truncates output.

Use interaction commands such as `click`, `fill`, `press`, navigation, viewport changes, or network interception only when the user asks for an interaction or the requested verification requires it. Explain any action that could alter application state before taking it.

## Safety boundaries

- Never run `agent-browser close` while attached to the user's Chrome; it may close their browser.
- Do not close tabs, clear storage, export state, inspect cookies, or save authentication data unless explicitly requested.
- Do not navigate unrelated tabs.
- Do not submit trades, payments, forms, messages, or other consequential actions during a verification-only task.
- Keep screenshots in `/tmp` unless the user requests a repository artifact.
- If the connection shows only `about:blank`, stop using that target. Re-enable live debugging, obtain approval in Chrome, and attach again.

## Completion report

Lead with whether the live attachment and requested verification succeeded. Include:

- the checked page, without exposing unrelated tabs
- what visibly rendered or failed
- whether page errors were reported
- a clickable link to the screenshot
- any interaction or area that remains unverified

Phrase an empty `errors` result as “No page errors were reported during this check,” not as proof that the application can never error.
