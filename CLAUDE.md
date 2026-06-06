# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Static single-page website for SPARK, a youth art workshop organization based in Northern Virginia. No build step, no framework, no package manager — plain HTML, CSS, and JS served directly.

## Running locally

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

Any static file server works. There is no build, compile, or install step.

## Architecture

**Single page** (`index.html`) with five anchor-linked sections in order: `#home`, `#workshops`, `#about` (Volunteering), `#gallery`, `#contact`.

**CSS** (`style.css`) uses a dark theme defined entirely through CSS custom properties in `:root`:
- `--primary` / `--primary-soft` — orange brand color (`#ff8b1a`)
- `--text`, `--muted` — foreground colors
- `--border` — `rgba(255,255,255,0.08)`, reused across cards, inputs, and the header
- Two responsive breakpoints: `900px` (grid collapses) and `720px` (mobile nav activates)
- `scroll-padding-top: 80px` on `html` offsets anchor targets below the sticky header — keep this in sync if the header height changes

**JS** (`script.js`) handles only two things:
1. Mobile nav toggle — adds/removes `.open` on `.site-nav` and syncs `aria-expanded` on the toggle button. The `.site-nav.open` CSS rule is scoped inside `@media (max-width: 720px)`.
2. Sets the `_next` hidden input on the contact form to `window.location.href` so formsubmit.co redirects back to the page after submission.

## External services

- **formsubmit.co** — handles contact form POST at `action="https://formsubmit.co/sparkmcleanhs@gmail.com"`. Activation requires a one-time email confirmation on first submission to a new address. Hidden fields `_next`, `_subject`, and `_honey` (honeypot) are already wired up.
- **Google Forms** — workshop sign-up links (`forms.gle/...`) in the workshop cards; these are external and not part of this repo.
- **Google Fonts** — Inter (400–800) and Montserrat (600, 700, 800). Font requests are in `index.html` line 9.
- **Unsplash** — all images are Unsplash CDN URLs with `auto=format&fit=crop`. The hero background is a CSS `background-image` (not an `<img>` tag). All `<img>` elements below the hero have `loading="lazy"`.
