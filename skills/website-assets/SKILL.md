---
name: "website-assets"
description: "Use when a website project needs visual assets (logo, photos, icons, illustrations, backgrounds, fonts, favicon, OG image; נכסים, תמונות, לוגו): ask per type with a recommendation, then source and log them."
---

# Website assets

The user decides; Claude lays out the options for each asset type with one recommendation, then sources, creates and logs the assets. Talk with the user in Hebrew; files, code and logs in English, client-facing text in Hebrew. Protect the user's time: one round of questions, minor choices decided by Claude and mentioned in the summary.

Order of preference for every asset type: what the client already has -> free and ready-made -> Claude writes it in code (SVG, CSS) -> AI generation -> a designer.

## When this runs
- Inside `client-website-workflow` stage 6 (content checklist), or in any website project when assets come up, including the user's own sites.
- Only for the asset types this site actually needs. Skip types already settled in `decisions.md` or already present in the project.

## Step 1: Inventory
From the sitemap or mockup, list the assets the site needs and what already exists (client files, old site, social profiles). Show it in a few lines.

## Step 2: Ask, per asset type
- Use AskUserQuestion when available: one question per asset type, up to 4 per call, the recommended option first and marked "(מומלץ)", each option one line with its cost and license note.
- Otherwise send one Hebrew message with numbered options per type and the recommendation marked, so the user can answer "הכל לפי ההמלצה".
- Ask once whether there is a budget for paid tools and who pays (the user or the client).
- Tool check before recommending: the tools named in this skill age fast (AI image tools change every few months). Search the web for the current pricing and commercial-use terms of the tools you are about to recommend, and for newer or better tools released recently. Present a strong newcomer as an extra option, with one line on why and its source. If a named tool has shut down, got worse terms or is no longer a common recommendation, tell the user with the source and offer to update this skill (remove or replace it). Change the skill only after the user approves. A few searches per project, not every message.

## Options by asset type

### Logo
- Client has one: get the original file (SVG, AI, EPS, PDF). If only a raster image exists, vectorize it (Recraft vectorize, or the open-source vtracer) and check the result by eye.
- No logo, small local business: a typographic logo (business name in a good Hebrew font, optionally one simple icon) that Claude builds as SVG. Free, fast, editable.
- Client wants a real brand: Recraft (true SVG; needs a paid plan for commercial rights), Ideogram for exploring directions, or a human designer if the logo matters to the client.
- Recommendation: typographic SVG logo by default; Recraft only when a real vector mark is needed; a designer when the brand matters.
- Checks before any logo becomes final:
  - Readable at favicon size (16px and 32px) and in one color (black, white); remove details that vanish or blur.
  - The SVG is real `<path>` shapes, not an embedded PNG: open the file as text and look for `<image` or `data:image`.
  - Replace any wordmark an AI tool drew with real text set in a free licensed font (see Fonts), then convert to paths. This matters most for Hebrew names.
  - Remind the user that no AI tool checks trademarks: a quick trademark search is theirs or the client's to do.

### Photos of the business (owner, team, work, vehicle, premises, products)
- Real photos from the client only. Send the client a short Hebrew photo guide, ready for WhatsApp: what to shoot, landscape orientation, daylight, clean background, several angles, permission from anyone who appears.
- Nano Banana (Gemini) may improve real photos: light, cropping, background cleanup. It must not invent or change what the work looks like.
- Recommendation: this is the most important asset for trust on a local business site; request it from the client as early as possible.

### Atmosphere, hero and background photos
- Free stock: Unsplash, Pexels, Pixabay (commercial use allowed, no attribution required; downside: seen on many sites).
- All three have free APIs: with an API key from the user, Claude can search and download candidates directly.
- Pixabay now holds many AI-generated images of uneven quality: check each image by eye, and do not use identifiable people in a context that looks like an endorsement.
- Unsplash and Pexels do not accept AI-generated uploads, so they stay the source for authentic photos of places and objects.
- Curated AI stock: Lummi (lummi.ai): hand-curated AI images, illustrations and 3D objects, free for commercial use with no attribution (redistribution limits apply). Looks less "seen everywhere" than classic stock.
- AI generation: Nano Banana by default; GPT Image as an alternative; Higgsfield if the user already has it.
- Recommendation: stock first (Unsplash/Pexels for authentic scenes, Lummi when the classic stock look is too generic); AI generation when something specific is needed and not found.

### Icons
- Open-source sets: Lucide (fits shadcn/ui), Tabler, Phosphor, Heroicons. Iconify aggregates many sets, but each set has its own license.
- Recommendation: Lucide, and only one icon set per site.
- When the site needs special icons (a distinctive style, or icons Lucide lacks), ask which route:
  - A free set from Iconstore (iconstore.co): check that set's license and that it covers every icon the site needs before choosing it.
  - The user sends an image of a style they like (from Iconstore or elsewhere), and Claude draws matching SVG icons. Works well for simple styles (line, geometric shapes); take the style only, never copy the icons themselves.
  - Recraft with the style image as reference, when many consistent icons or a complex illustrated style are needed (paid plan for commercial use).

### Illustrations
- unDraw (free, recolorable), Storyset (free tier requires attribution), Recraft for a consistent custom style.
- Recommendation: rarely needed on local business sites; real photos usually work better.

### Patterns, shapes and decorative backgrounds
- Claude writes them as SVG or CSS. Generators: Haikei (waves, blobs), Hero Patterns.
- Recommendation: Claude in code; free and fully controllable.

### Colors
- Order: colors from the client's logo or existing branding -> a palette the user found and liked (for example on Color Hunt, colorhunt.co) and sent to Claude -> a palette Claude proposes.
- Check every palette for WCAG AA contrast (IS 5568) on the combinations the site will actually use: body text on background, text on buttons, links. If a combination fails, propose the closest shade that passes and keep the rest of the palette as chosen.
- Realtime Colors (realtimecolors.com) previews a palette on a website layout with real proportions (60-30-10) and checks contrast; useful when the client needs to choose. It exports CSS, Tailwind and OKLCH.
- Sites with shadcn/ui (types 2 and 3): tune the theme (light and dark, radius, fonts) in tweakcn (tweakcn.com, free, open source) and copy its CSS variables.
- One accent color, used only for actions and meaning (call, WhatsApp, form buttons, links), never for decoration.
- The final palette goes into the design tokens in OKLCH (see `client-website-workflow` stage 6).
- Recommendation: the client's existing colors when they exist; otherwise the user's chosen palette, adjusted for contrast.

### Fonts (Hebrew)
- Free fonts only (Google Fonts or another OFL license). Never use paid or demo fonts, even if the client asks; offer a free alternative in the same spirit.
- Body and UI text: Heebo, Assistant. Hebrew and English in the same line with one character: Rubik, Open Sans. Long reading and a classic feel: Frank Ruhl Libre, David Libre. Full nikud: Noto Sans Hebrew.
- To avoid the generic Heebo/Assistant look, pair a readable body font with a free Hebrew display font for headings from Google Fonts (for example Secular One, Suez One, Bellefair, Karantina, Amatic SC). Check the current Google Fonts list filtered to Hebrew.
- Load only the weights in use (2-3, for example 400, 600, 700), with `font-display: swap` and a fallback stack.
- Recommendation: one or two families, self-hosted in the project rather than loaded from Google (faster and better for privacy); a clear hierarchy between headings and body text.

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
- Optimize: WebP by default, resized to the displayed sizes (let the framework's image pipeline, such as Astro Image, do it); AVIF for large hero images inside `<picture>` with a WebP fallback. Set `width` and `height` on every image (no layout shift); never lazy-load the hero (LCP) image, and give it `fetchpriority="high"`. Hebrew alt text for every meaningful image, and enough contrast for text placed on images.

## Hard rules
- Never generate fake photos of the client's own work, team, premises or customers, and never fake faces for testimonials.
- Never imitate another brand's logo or a known character, and never use photos of real people taken from the web.
- Output from a free plan without a commercial license is for exploration only, never for the delivered site.
- One icon set and at most two font families per site, and only free fonts.