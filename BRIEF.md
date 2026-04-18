# frenchbutnice.design — shared brief for variations

You are building ONE variation of Olivier Gillaizeau's personal site homepage. There are 10 parallel explorations total. Build only your assigned one, to production-grade quality, as a single self-contained HTML file.

## Who

**Olivier Gillaizeau.** French-born, San Francisco-based. Design systems leader.
- **Now:** Google Gemini (2022–present). Led the 5→1 design system unification, 607 teams on the system, 15K design hours saved annually, pioneered generative UI frameworks (Canvases).
- **Before:** Frame.io (2021–22, acquired by Adobe). Led Camera-to-Cloud launch + brand rebuild.
- **Before that:** Airbnb (2018–21). Global Creative Director for Guest Marketing — Only On Airbnb, Barbie Dreamhouse.
- 15+ years. Known for "systems as services, not mandates."

## What the site is

A **writer/speaker platform.** Thought leadership is primary; speaking engagements are the secondary ask.

- NOT a portfolio. NOT a job search asset. NOT a freelance pitch.
- Essays are the living layer. The work is the credential, not the pitch.
- Success = readers subscribe, quote, share, and eventually book Olivier to speak.

## The spine (two ideas only — everything else is noise)

1. **Design systems are organisms, not formulas.** Adoption over perfection. Coherence over uniformity. Systems as services, not mandates. Fifteen years of scaling systems across Airbnb, Frame.io, and Gemini.

2. **Designing with AI, not around it.** The practitioner view from inside Gemini: generative UI, Canvases, what it means for design systems to host models as first-class citizens. Most of the industry is still debating whether AI is coming; Olivier has been shipping it.

Within 10 seconds of landing, a reader should know these two things are what Olivier stands for.

## Voice

**Playful + precise.** Confident with a wink — the tension inside "French but Nice." Short sentences. No hedging. No thought-leader clichés ("in today's fast-paced world" — banned). Occasional French. The display type stays crisp and editorial; the wink lives in body copy.

## Information architecture (home page must contain all of this)

1. **Hero** — name + spine positioning (the two ideas, stated clearly)
2. **Latest essays** — 4–6 cards/links
3. **Speaking** — two topics, short invitation, book-me CTA
4. **About / credential line** — one short paragraph + "previously Gemini / Frame.io / Airbnb"
5. **Footer** — email, socials (Twitter/X, LinkedIn, RSS), copyright

No "Work," "Portfolio," or "Case Studies" nav item anywhere. Ever.

## Content to use (do not invent new titles — pick from these)

**Essay titles** (use 4–6 of these):
- Build for Adoption, Not Perfection
- Coherence Over Uniformity
- Systems Are Services, Not Mandates
- GenUI Before It Had a Name
- Why Your Design System Is a Liability
- Lessons from 607 Teams
- Models Don't Read Your Figma
- The Fragmentation Challenge
- Start Small, Scale Strategic
- Document the Why, Not Just the What

**Draft essay standfirst sentences** (use as preview text or hero copy):
- "The problem isn't that design systems are hard to build. It's that most of them were never meant to be lived in."
- "Systems are organisms, not formulas. I've spent fifteen years learning the difference — mostly the hard way."
- "Most teams still think AI means chat interfaces. I saw it generating custom UIs for every query, and moved my team to be ready before the demand hit."
- "Your design system is not a mandate. It is a service. Act accordingly."

**Speaking topics (exactly these two):**
- *Design Systems at Scale* — what breaks at 600+ teams, and how to build for adoption instead of enforcement.
- *Designing with Generative AI* — from Gemini's Canvases to the generative UI frameworks that change what a design system is for.

**Short bio (one-liner):** Designer. Systems lead. French, but nice.

**Long bio (~40 words):** Olivier Gillaizeau leads design systems at Google Gemini. Previously at Frame.io (acquired by Adobe) and Airbnb. Fifteen years shipping the infrastructure that makes creative teams faster without making them smaller.

**Credential line:** *Previously Gemini · Frame.io · Airbnb*

**Contact:** og@frenchbutnice.design

## Technical constraints

- **Single self-contained HTML file.** No separate CSS or JS files.
- **Tailwind via CDN is fine:** `<script src="https://cdn.tailwindcss.com"></script>` — or write vanilla CSS, whichever fits the variation.
- **Google Fonts for typography.** Pick real fonts, not system defaults. Typography is non-negotiable.
- **No build step.** No npm, no bundlers, no React. Vanilla JS only if needed.
- **Responsive.** Mobile (375px) and desktop (1440px) must both look intentional.
- **No images** — or if you must, use Unsplash source URLs or a single inline SVG.
- **Accessibility:** semantic HTML, sufficient contrast, visible focus states.
- Write your file to the exact path specified in your task. Use `mkdir -p` via Bash if the parent directory is missing.

## Quality bar

Real designers will look at these. Production-grade in spirit, not wireframes.

- Typography must be intentional (specific typefaces, real hierarchy, considered line-height).
- Color palette must feel composed, not default.
- Spacing must be editorial — generous, not cramped.
- The hero should land within 3 seconds on a cold load.
- No placeholder copy, no Lorem Ipsum, no "Coming Soon."
- No cookie banners, no carousels, no auto-playing video, no generic AI-design aesthetic (no purple gradients, no mesh blobs).

## Do not

- Explore any other part of the filesystem.
- Ask clarifying questions — build from what's in this brief.
- Write README files, documentation, or comments explaining the brief back at us.
- Build multiple pages. Home only.
- Invent new essay titles or a new spine.
- Reply with long explanations. Build the file and stop.

When done, return a one-line summary: what you built, what typefaces you used, what the key visual move is. That's it.
