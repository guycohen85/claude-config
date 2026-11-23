---
name: "client-website-workflow"
description: "Use when the user brings a client brief or spec for a website (אתר ללקוח, אפיון, בריף), or continues an existing client site project: intake, questions, tech options, scope and price, build with approval gates."
---

# Client website workflow

The user is an experienced developer who plans, decides and reviews; Claude writes all the code. These sites are side projects, often for friends or small local businesses, not the user's main work. Default to the lightest process and the lowest-maintenance result that meets the need, and protect the user's time: batch questions into one message, make minor decisions yourself, and keep messages to the user short. This workflow runs in Claude Code. Talk with the user in Hebrew. Documents for the client are in Hebrew, in plain non-technical language. `CLAUDE.md` and code are in English.

## Stage 0: Start or resume (every session)
- Client projects live in the user's clients folder on their computer (ask once if unknown; default `~/clients/`). One folder per client: `<clients-root>/<client-slug>/`.
- If the folder already has `decisions.md`, read it first, tell the user in one line which stage the project is at and what is open, and continue from there. Do not restart earlier stages.
- New project: create the folder, run `git init`, add a `.gitignore` (including `.env*`), and create `decisions.md` from the template below.
- Later, when the user asks, create a private GitHub repo with `gh` and push. Never commit secrets, passwords or client credentials.

Folder layout:
```
<client-slug>/
  decisions.md        # project state and decision log (Hebrew)
  CLAUDE.md           # generated in stage 6
  docs/client/        # client-facing Hebrew documents
  docs/internal/      # notes for the user only (estimates, prices)
  ...                 # site code, added from stage 7
```

`decisions.md` template:
```
# <client name>: החלטות
שלב נוכחי: <number and name> | ממתין ל: <user / client / nothing>
מסלול: מהיר / מלא
סוג אתר: <1/2/3> (זמני / מאושר)
רמה ויזואלית: נקי / מלוטש / מרשים
תקציב זמן (שעות שלך): <budget> | נוצל עד עכשיו: <used>

## שאלות פתוחות
## יומן החלטות
- <date> | <decision> | חלופות: <...> | למה: <...>
## שלבים שדולגו
## בקשות שינוי
| # | תאריך | בקשה | הערכה | מחיר | סטטוס |
```
Update it after every approved decision and at the end of every session (current stage and what it is waiting for). Commit after each update.

## Hard rules
- Work in stages. Move to the next stage only after a clear approval from the user (any clear yes, not a specific word). The user may skip a stage: do it, and log it under "שלבים שדולגו".
- Never write site code before the approval gate in stage 6.
- For each major technical decision (stack, hosting, CMS, auth, payments, data model) give 2-3 options with pros and cons and one recommendation. The user decides. Minor choices: decide, and mention them in the summary.
- No new libraries or services after stage 6 without approval.
- A request that arrives after scope approval is a change request: log it with an estimate and price suggestion, wait for approval, never add it silently.
- Prices are the user's decision. Claude suggests, the user sets.
- Fast track is the default: stages 4, 5 and 6 are presented together as one document with one approval. Use the full track only for type 2 or 3, or when the user asks.
- Time budget: every project has a budget for the user's own hours, recorded in `decisions.md`. Update the hours used at the end of each session. If the work is heading past the budget, stop and say so, with options: cut scope, a simpler stack, or pause.
- For the user's own site (no client), skip stages 2 and 5 and log the skip.
- Technology check: the tools named in this skill age. Before presenting options in stage 4 (stack, hosting, CMS, forms, visual-level libraries), search the web for the current state of the named tools and for newer or better alternatives released recently. Present a strong newcomer as an extra option, with one line on why it is worth considering and its source. If a named tool is deprecated, unmaintained, has worse license or pricing terms, or is no longer a common recommendation, tell the user with the source and offer to update this skill (remove or replace the tool). Change the skill only after the user approves. Keep it short: a few searches once per project, not every message.

## Companion skills (use if installed)
- `frontend-design`: the second stage 6 mockup (the first is made without it, see stage 6) and UI work in stage 7. Give it the visual direction approved with the client (for local service businesses usually trustworthy, clear and fast to contact), not its own bold default.
- `webapp-testing`: milestone checks in stage 7 and QA in stage 8: RTL, mobile layouts, forms, console errors, screenshots. Its scripts are ad-hoc checks; the project's permanent tests stay in TypeScript (Playwright).
- `code-review`: once the project is on GitHub, run it on each milestone pull request before asking the user to review.
- `website-assets`: the asset decisions in stage 6, and any asset that comes up later in the project.
- `scroll-craft` (third-party, optional): visual level 3 only, inside stages 6 and 7. Give it the approved brief, content and assets; do not let it run its own interview or process. Suggest the user review its scripts before installing it.
- If a companion skill is not installed, continue without it and mention once that it could help. Never install skills without asking.
- This skill owns the stages and approval gates. Do not let another workflow skill (for example Superpowers brainstorming or planning) take over the process; use such skills only inside a stage, if the user asks.

## Stage 1: Intake
Read the brief or spec (the client may not have one; then work from what the user tells you). Summarize in a few lines: what was understood, what is missing, the assumptions that would otherwise be made, a tentative site type (1/2/3), the suggested track, and a suggested time budget for the user's own hours. Ask the user to confirm.

## Stage 2: Client questions
Write `docs/client/01-questions.md` in Hebrew, plain language. About 6-8 required questions at most, then a few optional ones. With a friend this is often a quick call: the document doubles as a checklist the user fills in during the call. Use multiple-choice with a suggested default where possible, so the client can answer quickly (also give a short version that can be pasted into WhatsApp). Tailor it to the tentative type: do not ask a brochure-site client about permissions or integrations in depth. Cover:
- goal of the site, target audience, the main action a visitor should take (call, WhatsApp, form, booking, purchase)
- existing site or domain (to keep, migrate or redirect)
- pages and content; who writes the text and supplies photos and logo, and by when (ask early for real photos of the business and the original logo file)
- need for booking, payments, registration, admin area, integrations
- who edits content after delivery and how often
- local presence: Google Business Profile, service areas, WhatsApp button, reviews
- languages and RTL, accessibility, privacy and cookies, forms that collect personal details
- domain, hosting, email on the domain, analytics, existing accounts and branding
- timeline, budget range, examples of sites the client likes

## Stage 3: Gap check
The user brings the client's answers. Summarize the final requirements, list what is still missing or contradictory and the assumptions made. The user decides whether to go back to the client.

## Stage 4: Classify and choose technology
Classify as one of three types and explain why in a few lines.

Embed first: before escalating a type, check whether a hosted service covers the need: a payment page or link from a clearing provider, an embedded booking widget, a form service. Escalate only when the site itself must store user data, manage complex permissions or run business logic and integrations.

1. Static marketing site: Astro + Tailwind, content in Markdown, free static hosting (Cloudflare Pages or Netlify), contact form via Formspree or a small Worker. A single page with no expected growth can be plain HTML/CSS/JS. Add a light CMS (Decap, Keystatic or Sanity) only if the client edits content.
2. Site with database and users: Next.js, Postgres, Drizzle, managed auth (Better Auth, Clerk or Supabase Auth), Tailwind + shadcn/ui (Radix). Supabase is an all-in-one option. Host on Vercel, Cloudflare or Fly.io.
3. Complex system: TypeScript monorepo, frontend in Next.js or Vite + React, separate backend (NestJS, or Hono/Fastify), Postgres, Redis when needed, Docker, CI, automated tests and migrations.

Always include a no-code alternative (Wix, WordPress or similar) when the client wants to manage everything alone. For a friend with no maintenance arrangement, it is often the best answer. Prefer near-zero maintenance in every option: static hosting, no servers to patch, managed services, accounts in the client's name. Before recommending type 2 or 3, say plainly what it will cost the user in build time and ongoing support, and offer the simpler option first.

General principles: popular stacks with many public examples, TypeScript everywhere, no framework for a tiny single page, Tailwind and shadcn/ui over MUI, RTL from the start with logical CSS properties (`ms-`/`me-`, `start`/`end`). Run the technology check (Hard rules) and check current versions and setup before recommending. Present the recommendation with alternatives and wait for approval.

Visual level: the user chooses one of three levels together with the technology (use AskUserQuestion when available, the recommended level first). For each level, state the cost in the user's own hours and the effect on mobile performance. Recommend level 1 for local service businesses unless the client asked for more; levels 2 and 3 fit the user's own sites and clients who want a showcase.
1. Clean (default): CSS transitions and hover states only, no animation library.
2. Polished: a few effects per page: title reveals (GSAP SplitText) and elements entering on scroll (GSAP ScrollTrigger), or native CSS (scroll-driven animations, View Transitions) with a fallback for browsers without support.
3. Premium: scroll storytelling with GSAP ScrollTrigger and Lenis smooth scroll, using the `scroll-craft` skill if installed. Animated React components (React Bits, Aceternity UI, Magic UI) only when the stack is React, or Astro with React islands; check each library's license before using it on a client site. Motion (motion.dev) is an alternative to GSAP. References: Awwwards, Godly, Land-book, Codrops.

Motion rules for levels 2 and 3: honor `prefers-reduced-motion`; give a way to pause anything that moves for more than 5 seconds (IS 5568); keep all content readable without JavaScript; never animate the call, WhatsApp or form buttons; test motion direction in RTL; keep Lighthouse mobile performance at 90 or above. GSAP, including all its plugins, is free for commercial use (confirm in the technology check).

## Stage 5: Scope and agreement
Internal (`docs/internal/estimate.md`): milestones and the user's own time per milestone (their review, coordination and fixes; Claude does the build), checked against the time budget.

For the client (`docs/client/02-scope.md`, Hebrew, short): what is included and what is not, what the client supplies and by when, recurring costs (domain, hosting, services) in the client's name and paid by the client, and what happens with changes after delivery (none included, or a small defined allowance). Ask the user once whether there is a price (it may be free, symbolic or a favour); add price and payment terms only if there is one. No maintenance package by default; offer one only if the user wants it. Wait for approval.

## Stage 6: Design, content and plan (approval gate)
- Sitemap and page structure.
- Visual direction as simple static HTML mockups of the home page (`docs/mockup/`), openable locally, for the user and the client to approve before building. Always make at least two, as separate files:
  - `direction-a.html`: without `frontend-design`. Build it first, before loading that skill, so its guidance does not leak into this one.
  - `direction-b.html`: with `frontend-design`, loaded only after `direction-a.html` is done.
  - Further directions (`direction-c.html`, ...) are optional; say for each whether it used `frontend-design`.
  - When presenting the mockups, tell the user which file used `frontend-design` and which did not, and log it in `decisions.md`. Do not show this label inside the mockup pages, since the client sees them.
  - If `frontend-design` is not installed, tell the user before making the mockups and ask whether to install it or make both without it.

  Share them through the preview deploy (Cloudflare Pages, or GitHub Pages for a public repo). The approved mockup becomes the starting point of the site. Do not use Claude Design for the site mockup; it is for marketing graphics (see `website-assets`).
- Visual level 2 or 3: include the motion in the mockup (the hero and one or two sections), so the user approves the feel before the build.
- Content checklist (`docs/client/03-content.md`): every text, image and asset needed, who supplies it and by when. Missing content does not block the build: use clearly marked placeholders (`[חסר: ...]`).
- Assets: run the `website-assets` skill for the asset types the site needs (logo, photos, icons, fonts, favicon and Open Graph image). It asks the user per type with options and a recommendation; its decisions go into the content checklist and `decisions.md`.
- Data model, for types 2 and 3.
- Generate `CLAUDE.md` from `decisions.md`: approved stack and versions, folder structure, conventions, RTL rules, "no new libraries without approval", how to run, test and deploy, and a pointer to `decisions.md` as the source of truth for decisions. Update the relevant part whenever a decision or change request changes it.

Summarize the plan and ask for explicit approval to start building.

## Stage 7: Build in milestones
- One milestone at a time, on a branch, committed when done.
- After each milestone: run the build and the tests, check the pages with `webapp-testing` if installed (RTL, mobile, forms), run `code-review` on the pull request once on GitHub, start the dev server so the user can look locally (once the project is on GitHub and connected to hosting, give the preview deploy link instead), and give a short summary: what was built, what to check, what is still a placeholder.
- Wait for the user's review before continuing.
- Tests: type 1, a passing build and a link check; types 2 and 3, Playwright smoke tests for the critical flows plus unit tests for business logic.

## Stage 8: QA and handoff
Checklist (use `webapp-testing`, if installed, for the mobile, RTL and forms checks):
- mobile, RTL, cross-browser, performance (Lighthouse), forms work end to end, 404 page, redirects from the old site; for visual levels 2 and 3, the motion rules from stage 4 (reduced motion, pause control, content without JavaScript)
- SEO: titles and meta descriptions, Open Graph tags and image, favicon and app icons, `sitemap.xml`, `robots.txt`, Google Search Console, link to Google Business Profile where relevant, analytics if agreed
- accessibility: the site itself must meet the Israeli standard (IS 5568, based on WCAG AA), not only show an accessibility statement; an overlay widget does not replace this. Add the accessibility statement page.
- privacy: privacy policy, cookie notice if trackers are used, and attention to Israeli privacy law for forms that collect personal details. Remind the user that Claude is not a lawyer and the current requirements should be checked.
- backups (where there is a database or CMS), domain and hosting registered in the client's name

The goal is that the client does not need the user for routine changes. Write `docs/client/04-handoff.md` in Hebrew: how to edit content, where things are hosted, who owns which account, recurring costs and renewal dates, maintenance notes, and how to request changes. Never put passwords in this document: tell the user to hand them over through a secure channel. Mark the project as delivered in `decisions.md`.