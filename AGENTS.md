# AGWS.md → AGENTS.md

## Overview
Static Chinese coursework portfolio (高级网页设计 / Advanced Web Design, student 魏鹏). Pure HTML + CSS, native code only — no frameworks, no CDN, no build step, no dev server, no tests. Open `index.html` in a browser to run.

## Conventions
- `index.html` is the required entry page (per the assignment quoted in the file itself): it must link to every page in this directory. When adding a page, also add a card for it there.
- **Every newly created HTML page must be added as a hyperlink/card in `index.html`** — no orphan pages.
- File naming is `<lesson>-<exercise>.html` (e.g. `3-1form.html`, `10-5.html`); some exercise names are Chinese (`5-1层叠与继承.html`, `7-1上图下文.html`).
- Chinese/space-containing filenames must be percent-encoded in `href` (spaces as `%20`, Chinese chars as UTF-8 percent-encoding) — see the existing links in `index.html`.
- Almost every page is fully self-contained with inline `<style>`; only `5-1层叠与继承.html` links the shared `css.css`. Do not add new shared stylesheets without reason.
- Images live in `img/`; coffee-project assets in `咖啡/`.

## Naming Rules
- All CSS class names, id names, and JS variables must start with `魏鹏_` (e.g. `魏鹏_topbar`, `魏鹏_vpW`, `魏鹏_root`).

## Prompt Logging
- Before executing any task, append the prompt to `魏鹏_prompt.md` (create if missing, never overwrite existing content).
- Each entry must include: timestamp (`yyyy-mm-dd-hh-mm`), model used, and the full prompt content.

## Workflow
- Edit HTML directly; verify by opening pages in a browser. No lint/typecheck commands exist.
