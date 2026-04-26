---
name: github-repo-to-html-study-site
description: Research a GitHub/open-source project from official docs, source, and tests, then produce a local file://-compatible multi-page HTML study site with a launcher, cross-page navigation, detailed subsystem analysis, and embedded code explanations.
triggers:
  - User gives a GitHub repo or docs link and wants a study website
  - User wants source-backed technical learning content in HTML form
  - User wants a local offline-readable analysis package under a folder
  - User asks to turn project analysis into a reusable study module
version: 1.0.0
---

# GitHub Repo → HTML Study Site

Use this skill when the user gives you a repository/docs link and wants a detailed local HTML learning site, not just a chat summary.

The output is a foldered study module that:
- can be opened directly via `file://` by double-clicking `index.html`
- also works when served over `http://`
- includes a root/launcher page or module registration entry when appropriate
- splits content into multiple topic pages
- keeps a shared left-side page list on every page
- explains important code *inside the relevant topic page*, not only in one isolated code page
- is grounded in official docs, source files, and tests

## Core quality bar

Do **not** stop at README-level summaries.

A good deliverable must include:
1. project identity confirmation
2. subsystem-oriented structure
3. source-backed technical claims
4. embedded code explanations near the relevant topic
5. engineering trade-offs, not just features
6. local reading ergonomics: navigation, launcher, readable layout, file:// compatibility

## When to use official sources

Always prefer, in this order:
1. official docs site
2. repo docs (`docs/**`, `website/docs/**`)
3. source files for implementation details
4. tests for invariants, caps, edge cases, and regression intent
5. README for orientation only

If the user asks about optimization, memory, persistence, budgeting, boundaries, or safety, reading tests is mandatory.

## Deliverable architecture

Default structure:

```text
TARGET_DIR/
├── index.html                  # launcher or study entry
├── manifest.json               # optional launcher/module registry
└── <study-slug>/
    ├── index.html
    ├── overview.html
    ├── architecture.html
    ├── <topic>.html
    ├── <topic>.html
    └── assets/
        ├── style.css
        ├── app.js
        └── manifest.json
```

If the target root already acts as a multi-study workspace, preserve that pattern and register the new module there.

## Required UX rules

### 1. Every page must support switching to the others
Every study page must have a shared navigation list linking to all sibling topic pages.

### 2. File protocol compatibility is required
Do **not** rely only on `fetch('./manifest.json')` or similar local JSON fetches for critical bootstrapping.

Because users often double-click local HTML files on Windows, `file://` pages may fail CORS fetches. Use one of these patterns:
- embed launcher/study manifest as `window.*` inline JSON in the page, with `fetch` only as an http/https fallback
- or render static nav directly in each page if the module is simple

If you use JS bootstrapping, support both modes:
- `file://` → inline manifest data
- `http://` / `https://` → optional fetch allowed

### 3. Code explanations belong in the relevant section
Do not isolate all code commentary into a single “code explanations” page.

Examples:
- prompt budget code goes in the context-budget page
- session trimming code goes in the session/history page
- versioning code goes in the GitStore/versioning page

A separate code-reading map page is allowed, but it must be secondary.

### 4. Content must be detailed enough to study from
Avoid thin pages with only 2–4 generic paragraphs.

Each substantive page should usually contain:
- what problem this subsystem solves
- how it fits in the larger pipeline
- one or more important code snippets
- explanation of what the code is doing
- why the design likely exists
- failure modes / trade-offs / limits
- what is worth reusing in another project

## Recommended page set

Adapt to the project, but for technical OSS study sites this default often works well:
- `index.html` — study entry and reading path
- `overview.html` — project identity and problem definition
- `architecture.html` — system/data flow
- one page per important subsystem
- `optimization-summary.html` — reusable takeaways
- `sources.html` — explicit evidence list

Optional:
- `code-walkthrough.html` as a reading map, not the sole place for code analysis

## Workflow

### Step 1: Confirm the project identity
Do not assume the first repo match is correct.

Check:
- repo owner/org
- official docs link alignment
- README/repo description relevance
- stars only as weak evidence
- source tree contains the subsystem the user asked about

### Step 2: Build a source map before writing
Collect:
- key docs pages
- main source files
- matching tests
- any templates/configs tied to the subsystem

Record exact file paths.

### Step 3: Extract subsystem facts
For each subsystem, pull out:
- responsibilities
- key data structures/files
- caps/thresholds/limits
- fallback behavior
- cursor/state progression
- where correctness is enforced
- where the tests reveal intended invariants

### Step 4: Design the HTML information architecture
Create a page map before writing content.

For each page define:
- page title
- one-sentence scope
- major sections
- which code snippets belong there

### Step 5: Write content with embedded evidence
For each page:
- explain in plain language first
- then show the relevant code block
- then explain the code’s role and implications
- distinguish documented facts from inference

### Step 6: Build the local site shell
Create:
- shared CSS
- shared app JS if useful
- manifest structure
- left navigation on every page
- launcher page if root is multi-study

Prefer a clean technical reading UI over flashy effects.

### Step 7: Verify in both content and runtime dimensions
Check:
- files exist in the intended folder
- all nav links are correct
- study pages render when opened directly via `file://`
- no JS errors in browser
- content claims are source-backed
- page set is internally consistent

## Research guidance

### Read tests when the user asks “how is X optimized?”
Tests often reveal:
- edge cases
- oversized-input handling
- replay/cursor bugs
- expected fallback semantics
- what the maintainers consider a safe boundary

### Use the Git tree API for file discovery
For large repos:
- use the recursive git tree API to map candidate files quickly
- then fetch raw file contents from `raw.githubusercontent.com`

### Prefer implementation-level language
Bad:
- “The system manages memory efficiently.”

Good:
- “The system caps archived summaries at N chars, truncates prompt previews before model calls, and advances a cursor after fallback writes to prevent duplicate replay.”

## HTML writing guidance

Use a durable, readable structure:
- dark or neutral technical theme is fine
- strong typographic hierarchy
- code blocks with clear spacing
- card/callout components for trade-offs and key takeaways
- responsive layout

Recommended content pattern inside each page:
1. problem statement
2. subsystem role
3. key code snippet(s)
4. explanation of behavior
5. why this design exists
6. risks / trade-offs
7. takeaway for reuse

## Pitfalls to avoid

### Thin pages
Do not produce a multi-page site where each page has only shallow summary paragraphs.

### README-only analysis
Do not present internal behavior as fact unless it is backed by source/tests/docs.

### Centralized code dump
Do not put all code explanation into one code page and leave the topic pages generic.

### Broken file:// mode
Do not depend on local JSON fetch for initial render without an inline fallback.

### Weak navigation
Do not make the user return to the launcher to switch pages. Every page must link to all peers.

### Overwriting an existing study workspace structure
If the target root already has a launcher/manifest/module convention, extend it rather than replacing it.

## Verification checklist

Before finishing, verify all of the following:
- [ ] correct repo identity confirmed
- [ ] docs + source + tests used where relevant
- [ ] topic pages are detailed enough to learn from
- [ ] code explanations embedded in relevant pages
- [ ] every page has sibling-page navigation
- [ ] launcher works if the workspace uses one
- [ ] `file://` open works without fetch CORS failure
- [ ] browser console is clean
- [ ] source/reference page included

## Suggested final handoff wording
Include:
- where the files were written
- which pages are most important to read first
- whether the site works via direct double-click
- what part could be expanded next (for example, deeper per-function walkthroughs)

## Reusable lessons encoded from prior iterations
- Users often want a long-lived learning workspace, not a one-off HTML file.
- Root launcher + per-study module is a good default for a growing `study-web` folder.
- Inline manifests plus optional fetch fallback avoid file:// CORS failures.
- If the user says the content is too thin, expand each subsystem page, not only the overview.
- If the user says code explanation should not be isolated, keep the explanations inside the relevant subsystem page and demote the standalone code page to a reading map.
