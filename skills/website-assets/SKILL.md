---
name: "website-assets"
description: "Use when a website project needs visual assets (logo, photos, icons, illustrations, backgrounds, fonts, favicon, OG image; נכסים, תמונות, לוגו): ask per type with a recommendation, then source and log them."
---

# Website assets

The user decides; Claude lays out the options for each asset type with one recommendation, then sources, creates and logs the assets. Talk with the user in Hebrew; files, code and logs in English, client-facing text in Hebrew. Protect his time: one round of questions, minor choices decided by Claude and mentioned in the summary.

Order of preference for every asset type: what the client already has -> free and ready-made -> Claude writes it in code (SVG, CSS) -> AI generation -> a designer.

## When this runs
- Inside `client-website-workflow` stage 6 (content checklist), or in any website project when assets come up, including the user's own sites.
- Only for the asset types this site actually needs. Skip types already settled in `decisions.md` or already present in the project.

## Step 1: Inventory
From the sitemap or mockup, list the assets the site needs and what already exists (client files, old site, social profiles). Show it in a few lines.

## Step 2: Ask, per asset type
- Use AskUserQuestion when available: one question per asset type, up to 4 per call, the recommended option first and marked "(מומלץ)", each option one line with its cost and license note.
- Otherwise send one Hebrew message with numbered options per type and the recommendation marked, so he can answer "הכל לפי ההמלצה".
- Ask once whether there is a budget for paid tools and who pays (the user or the client).
- Tool check before recommending: the tools named in this skill age fast (AI image tools change every few months). Search the web for the current pricing and commercial-use terms of the tools you are about to recommend, and for newer or better tools released recently. Present a strong newcomer as an extra option, with one line on why and its source. If a named tool has shut down, got worse terms or is no longer a common recommendation, tell the user with the source and offer to update this skill (remove or replace it). Change the skill only after the user approves. A few searches per project, not every message.

## Options by asset type

### Logo
- Client has one: get the original file (SVG, AI, EPS, PDF). If only a raster image exists, vectorize it (Recraft vectorize, or the open-source vtracer) and check the result by eye.
- No logo, small local business: a typographic logo (business name in a good Hebrew font, optionally one simple icon) that Claude builds as SVG. Free, fast, editable.
- Client wants a real brand: Recraft (true SVG; needs a paid plan for commercial rights), Ideogram for exploring directions, or a human designer if the logo matters to the client.
- Recommendation: typographic SVG logo by default; Recraft only when a real vector mark is needed; a designer when the brand matters.

### Photos of the business (owner, team, work, vehicle, premises, products)
- Real photos from the client only. Send the client a short Hebrew photo guide, ready for WhatsApp: what to shoot, landscape orientation, daylight, clean background, several angles, permission from anyone who appears.
- Nano Banana (Gemini) may improve real photos: light, cropping, background cleanup. It must not invent or change what the work looks like.
- Recommendation: this is the most important asset for trust on a local business site; request it from the client as early as possible.

### Atmosphere, hero and background photos
- Free stock: Unsplash, Pexels, Pixabay (commercial use allowed, no attribution required; downside: seen on many sites).
- AI: Nano Banana by default; GPT Image as an alternative; Higgsfield if the user already has it.
- Recommendation: stock first; AI when something specific is needed and not found.

### Icons
- Open-source sets: Lucide (fits shadcn/ui), Tabler, Phosphor, Heroicons. Iconify aggregates many sets, but each set has its own license.
- Recommendation: Lucide, and only one icon set per site.

### Illustrations
- unDraw (free, recolorable), Storyset (free tier requires attribution), Recraft for a consistent custom style.
- Recommendation: rarely needed on local business sites; real photos usually work better.

### Patterns, shapes and decorative backgrounds
- Claude writes them as SVG or CSS. Generators: Haikei (waves, blobs), Hero Patterns.
- Recommendation: Claude in code; free and fully controllable.

### Fonts (Hebrew)
- Google Fonts with Hebrew support: Heebo, Assistant, Rubik, Frank Ruhl Libre, Noto Sans Hebrew (free, commercial use allowed).
- Recommendation: one or two families, self-hosted in the project rather than loaded from Google (faster and better for privacy).

### Favicon and Open Graph image
- Favicon and app icons: generated from the logo (RealFaviconGenerator, or in code).
- Open Graph image: generated in code (logo, title, brand color), designed in Claude Design, or made with Nano Banana.
- Recommendation: both in code from the logo.

### Marketing graphics (social posts, WhatsApp status images, flyers, business cards, banners, ads)
- Claude Design (the Design artifact type in Claude): a canvas of editable artboards, several variants side by side, comments, and export to PDF or image. Real photos and AI images go into it as uploads.
- Canva, when the client wants to edit and reuse the graphics alone.
- Recommendation: Claude Design. For a client who will need many marketing outputs, also record the brand once as a Design System artifact (colors, fonts, logo) so every graphic matches the site.
- Not for the site mockup: that stays an HTML mockup in the repo (see `client-website-workflow` stage 6).

### Video (rare)
- Pexels videos, or AI video (Higgsfield, Veo, Kling) only if the client asks.
- Recommendation: usually skip; hero video hurts performance on mobile.

## Tools reference
- Nano Banana (Google Gemini image models): raster only, strong at editing real photos, photoreal images and text in images. Images made in the Gemini app may carry a visible Gemini watermark: check before using them on a client site, or use the API.
- Recraft: vectors and SVG, logos, icons, vectorizing, consistent custom styles. Free-plan images are public, owned by Recraft and not licensed for commercial use, so client work needs a paid plan; a one-month subscription is enough, and ownership of images made while subscribed stays after cancelling. Remote MCP: https://mcp.recraft.ai/mcp.
- Higgsfield: a hub of image and video models (including Nano Banana and GPT Image) with an MCP; worth it when the user already has an account or video is needed.
- Claude Design: layouts and marketing graphics, not image generation and not final vector logo files. It lives in Claude (web and desktop app); if its tools are not available in this session (for example in Claude Code), give the user a ready brief to paste into a Claude chat.
- Others: GPT Image (ChatGPT), Ideogram (text in images, logo directions), Midjourney (aesthetics, less suited to automation), Canva (when the client wants to edit banners and posts alone).
- Use a tool through MCP only if it is connected. Never install an MCP server or sign up for a service without asking, and ask before spending paid credits.

## Step 3: Execute and log
- Record the decision per asset type in `decisions.md` when it exists, one line each.
- Keep assets in the project's assets folder (for example `src/assets/`), with `assets/SOURCES.md`: file, source, license or terms, link or author, prompt if AI-generated, date.
- Stock: download the chosen images and keep the link to the original page.
- AI: generate through a connected MCP. If none is connected, write ready-to-use prompts in `docs/internal/asset-prompts.md` and keep placeholders.
- Missing assets do not block the build: use clearly marked placeholders (`[חסר: ...]`) and list them in the content checklist.
- Optimize: WebP or AVIF in the right sizes (let the framework's image pipeline, such as Astro Image, do it), Hebrew alt text for every meaningful image, and enough contrast for text placed on images.

## Hard rules
- Never generate fake photos of the client's own work, team, premises or customers, and never fake faces for testimonials.
- Never imitate another brand's logo or a known character, and never use photos of real people taken from the web.
- Output from a free plan without a commercial license is for exploration only, never for the delivered site.
- One icon set and at most two font families per site.