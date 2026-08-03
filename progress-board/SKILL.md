---
name: progress-board
description: Render a visual, AuDHD-friendly status board of the current session or project — what's done, what's broken, what's waiting on the user, what's next. Use when the user says "progress board", "status board", "show me progress", "visual report", "where are we", or invokes /progress-board.
---

# Progress board

Render the current state of work as ONE visual widget the user can scan in
seconds. The user is AuDHD: grouping, color, counts, and short rows beat
prose. Never deliver this report as paragraphs.

## Gather (before rendering)

Pull real state — never invent numbers:
- `git log --oneline` for recent commits (short hashes go on the rows)
- test suite result if one exists (count, pass/fail)
- the conversation itself: what was found, fixed, deferred, decided
- any plans/TODO index in the repo (e.g. `.claude/plans/README.md`, TASKS.md)

## Render

If the `mcp__visualize__show_widget` tool is available: call
`mcp__visualize__read_me` first (silently, modules: mockup), then build ONE
HTML widget with this structure:

1. **Metric cards row** (2–4 cards): version, commits, tests green, items
   fixed/open. Big number, muted 13px label.
2. **One card per section**, each with a header row: Tabler icon + title +
   right-aligned count pill ("N of M fixed" / "N open"). Sections in this
   order, skip empty ones:
   - Done / fixed — green (`--text-success`, `--bg-success`), `ti-check`
     rows, commit hash right-aligned in `<code>` when known
   - Waiting on user — amber (`--text-warning`), `2px solid
     var(--border-warning)` card border, `ti-square` checkbox rows
   - Blocked / broken — red (`--text-danger`), `ti-alert-triangle`
   - Next options — blue (`--text-accent`), each row gets a small
     `sendPrompt('...')` button ("Start ↗") so one click kicks off the work
3. Style rules: CSS variables only (dark-mode safe), flat, no emoji (Tabler
   outline icons), sentence case, one line per row (≤12 words), 14px rows.

If the visualize tools are NOT available: same structure as markdown — a
stats line, then one `###` section per group with checkbox lists
(`- [x]` done, `- [ ]` waiting) and counts in the headers. Still no prose
paragraphs.

## After the widget

One or two sentences max below the widget: what the colors mean and the
single most important next action. Do not repeat the board's content in text.
