# Build Log

A running record of real decisions, errors, and fixes made while building
this app with Claude Code. Kept so it's easy to write an assignment memo
afterward without having to reconstruct what actually happened.

## Decisions

- **Where the project lives:** this repo (`entp4332`) already held two
  unrelated demo folders (`Session2_BadPrompt`, `Session2_DetailedPrompt`)
  comparing prompt quality, with a `CLAUDE.md` describing that as the repo's
  purpose. Office Hours Prep is a different assignment, so I was asked to
  choose between a brand-new separate repo or a subfolder here. I chose a
  **subfolder inside the existing repo** (`OfficeHoursPrep/`), sharing the
  same GitHub repo and git history as the other two folders.
- **Which fields are required:** *Topic/assignment*, *Where I am stuck*, and
  *What I want to understand after office hours* are required. *Course name*,
  *What I understand so far*, and *What I have already tried* are optional,
  since a student might legitimately not have attempted anything yet.
- **No AI-generated content, anywhere, including the added "questions"
  feature.** When asked to add suggested questions to ask during office
  hours, the temptation is to generate them contextually from the student's
  specific problem — but that would require an AI API, which was explicitly
  ruled out. Instead, the questions are a **fixed list**: two are built by
  quoting the student's own "stuck" and "goal" text back into a question
  frame (so they feel tailored without inventing anything), and four are
  generic, always-applicable prompts written directly into the code.
- **Single self-contained `index.html`**, no framework, matching the existing
  repo convention (`CLAUDE.md`: "each `index.html` is fully self-contained").

## Errors and fixes

- **HTML entity leaking into a form field.** The first draft of the "Try an
  example" sample data used `&mdash;` (an HTML entity) inside a plain
  JavaScript string that gets assigned to a `<textarea>`'s `.value`. Since a
  textarea's value is plain text, not HTML, this would have literally shown
  the text `&mdash;` instead of an em dash (—). Fixed by using the actual
  em-dash character directly in the JavaScript string, and by removing a
  clumsy `.replace()` workaround that was added as a first attempt before the
  root cause was clear.
- **No JavaScript runtime available to lint/check the script.** This
  environment has no `node`/`npx`. Worked around it by manually reviewing the
  script for balanced braces/quotes, then verifying behavior by actually
  running the app in a browser (see Testing, below) rather than relying on a
  syntax checker.
- **Browser automation was mostly unavailable in this environment.** The
  Claude-in-Chrome extension was declined. AppleScript-driven Safari/Chrome
  automation failed with `Not authorized to send Apple events` (macOS
  Automation/TCC permission not granted, and it can't be self-granted without
  a GUI prompt). No Playwright/Selenium installed. Worked around this by
  launching headless Chrome directly (`--headless=new
  --remote-debugging-port`) and writing a small Chrome DevTools Protocol
  client from Python's standard library only (raw WebSocket handshake and
  framing — no third-party packages) to actually click buttons, fill fields,
  handle the `confirm()` dialogs, and capture real screenshots.
- **Clipboard write denied in the test environment.** Testing the Copy button
  under headless Chrome on a `file://` URL, `navigator.clipboard.writeText()`
  was rejected with `NotAllowedError: Write permission denied` — a real
  browser permission restriction for local files, not a bug. This correctly
  exercised the app's fallback copy path (`document.execCommand('copy')`),
  which worked. Because the deployed app will be served over `https://` from
  Vercel rather than `file://`, the primary clipboard API should work there
  without needing the fallback — worth a quick manual check on the live site.

## Testing performed before shipping

- Empty-form submit → correct required-field error messages, correct fields
  flagged.
- "Try an example" → "Create My Prep Sheet" → outline renders all six answers
  verbatim, plus the six starter questions.
- Only required fields filled (optional ones left blank) → outline correctly
  shows "Not provided." for the blank optional fields.
- Whitespace-only text (e.g. a lone space) in a required field → still
  correctly rejected as missing.
- HTML/script-like input (`<script>alert(1)</script>`, etc.) in the form →
  rendered as plain escaped text in the outline, not executed.
- Print-media emulation → header, form, and all buttons correctly hidden;
  only the outline prints.
- Edit answers → form reappears with previously typed values intact.
- Start over → confirmation prompt appears, then form fully clears.
- Mobile viewport (390×844) → no horizontal scrolling, form and outline both
  readable.
