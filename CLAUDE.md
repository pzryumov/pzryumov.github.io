# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository purpose

Personal academic website for Pavel Zryumov (Assistant Professor of Finance, Simon Business School, University of Rochester). Served via GitHub Pages from the `pzryumov.github.io` repo — `master` is published directly as the live site. There is no build step, no package manager, and no test suite.

## Editing and previewing

Content lives in three sibling pages: `index.html` (Research, the landing page), `teaching.html`, and `discussions.html`. To preview locally, open the relevant HTML file directly in a browser, or serve the directory statically (e.g. `python3 -m http.server` in the repo root) and visit `http://localhost:8000`. Deploy by committing to `master` and pushing — GitHub Pages picks it up.

## Architecture

- **`index.html`** — landing page (Research). Sections (in order): header/nav, bio jumbotron, Publications, Working Papers, Work In Progress, Contact Information. This is also the only page that runs the "New" badge script.
- **`teaching.html`** — Teaching page. Each course is a `<ul>` with two `<li>` items: the course header line, and a `<li class="class-description">` paragraph.
- **`discussions.html`** — Discussions page. Each entry is a `.discussion` block with `.number`, `.discussion-conference`, `.title`, and `.authors` spans, numbered in reverse-chronological order (newest = highest number).
- **`css/bootstrap.css`** — vendored Bootstrap 3 (do not edit; grid classes like `col-md-*`, `hidden-xs`, and glyphicons come from here).
- **`css/main.css`** — all site-specific styling. Selectors are scoped by section class (`.publications`, `.working-papers`, `.publication .title`, `.working-papers .abstract`, `.discussion`, `.teaching`, `.nav-link`, etc.). When adding a new kind of entry, reuse an existing class pattern so styling flows through automatically.
- **`files/`** — publicly linked PDFs (CV at `files/pavelzryumov_cv.pdf`; link is hard-coded in all three pages' nav). Replace the PDF in place to update the CV; no other change needed.
- **`img/`, `fonts/`** — static assets. Multiple photo sizes exist for responsive rendering.

## Conventions to preserve when editing

- **Shared nav across pages**: the `<div class="header">` block (Simon logo + Discussions/Teaching/CV/Research links) is duplicated verbatim in `index.html`, `teaching.html`, and `discussions.html`. When adding/renaming a page or reordering links, update all three so navigation stays consistent. On `index.html` the logo is a plain `<img>`; on the subpages it's wrapped in `<a href="index.html">` so the logo links home.
- **Duplicated analytics**: both Google Tag Manager (`GTM-MC9XRFW7`, in `<head>` plus a `<noscript>` iframe) and legacy Google Analytics (`UA-52398032-1`, inline script at the bottom of `<body>`) appear on every page. Keep both, and keep them in sync across pages unless the user explicitly asks otherwise.
- **"New" badge automation**: the inline script at the bottom of `index.html` parses every `<span class="date">…</span>`, treats the text as a `Date`, and tags it with `id="new"` if it is within ~360 days. For this to work, date spans must contain a browser-parseable date string (e.g., `February 2026`, `August 2025`). Don't wrap date text in other elements. The script only runs on `index.html` — `teaching.html` and `discussions.html` have no `.date` entries and don't need it.
- **Working paper entry shape**: `.working-paper` blocks follow a consistent skeleton — `.title` (with link), `.co-authors`, optional `.paper-link` (submission status like "R&R at the RFS"), `.date`, and `.abstract`. Match this structure when adding a new paper so CSS spacing and the "new" script apply correctly.
- **Email obfuscation**: the mailto links and displayed email are written as HTML numeric character entities (`&#112;&#097;…`). Preserve this obfuscation — don't replace with plain text when editing surrounding markup.
- **Doctype**: all pages use the legacy HTML 4.01 Transitional doctype. Don't "modernize" to HTML5 as a drive-by change — keep edits scoped to what's requested.
