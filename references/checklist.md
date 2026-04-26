# HTML Study Site Module Checklist

Use this when executing the `github-repo-to-html-study-site` skill.

## Discovery
- Confirm official repo identity
- Confirm official docs site if present
- Enumerate relevant docs, source files, and tests
- Note exact file paths for every technical claim

## Content quality
- Every important page explains one subsystem deeply
- Code snippets appear inside the relevant topic page
- Include trade-offs, limits, and edge-case behavior
- Use tests when discussing optimization, caps, fallback, replay, migration, or correctness

## Site structure
- Root launcher if workspace is multi-study
- Per-study folder with local assets
- Shared navigation on every page
- Sources page listing docs/source/tests

## Runtime compatibility
- Must work via `file://` direct-open
- Do not require local JSON fetch without inline fallback
- If JS bootstraps content, embed `window.STUDY_MANIFEST` / `window.LAUNCHER_DATA`
- Verify no browser console errors

## Final verification
- Open launcher
- Open one study page directly
- Switch between sibling pages
- Confirm content is detailed enough to study from, not just skim
