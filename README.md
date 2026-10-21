# CLAUDE.md - cypruspropertyadvisor.com

## What this repo is
The WordPress build for cypruspropertyadvisor.com: a custom block theme (/theme) and a small site plugin (/plugin).
The owner, Benjamin, is a consultant, not a developer. Explain every step in plain language and give click-by-click instructions for anything he must do in wp-admin, Cloudways or Cloudflare.

## Read first
- /docs/master-brief.md: business, audience, rules. Wins over everything else.
- /docs/seo-spec.md: URLs, Rank Math, schema, Linguise, redirects, GA4 events (from Project 1).
- /docs/page-map.xlsx: every page with ID, slugs EN/NL, template, links, schema.
- /docs/brand/: brand book, tokens.json, theme.json fragment, component inventory, assets.
- /docs/prototypes/: one HTML prototype per template. data-pattern names are the block pattern names.
- /content/: approved copy in Markdown, one file per page ID.

## Stack
- WordPress (latest stable), PHP 8.2+, on the new 2 GB Cloudways server: one staging app, one production app.
- Domain and DNS on Cloudflare (proxied, SSL Full strict).
- Plugins allowed without asking: Rank Math Pro, Linguise, one cookie-consent plugin with Google Consent Mode v2, one two-factor login plugin. No page builders (Elementor, Bricks, Divi), no GeneratePress. Anything else: ask first and explain why.
- Services: TidyCal (booking), Kit (email), GA4 and Search Console, Cloudflare Turnstile (spam protection).
- Local development with wp-env if Docker is available; otherwise test on staging.

## Repo structure
- /theme: block theme cpa (theme.json, templates, parts, patterns, styles, assets with self-hosted woff2 fonts and SVG icons)
- /plugin: cpa-site (post types, custom blocks, schema helpers, REST fields, integrations)
- /content: page copy from Project 3
- /docs: specs and design files, read-only unless asked
- /scripts: WP-CLI and deploy helpers
- CHANGELOG.md, README.md, .env.example. Never commit .env or any password.

## Build rules
- Design tokens go into theme.json exactly: palette, font families, fluid font sizes, spacing scale, contentSize and wideSize. No hard-coded colours or sizes in CSS.
- Every prototype section becomes a block pattern with the same name (cpa/...). Use core blocks; custom blocks only for the intake form, calculators, project facts table, table of contents and gallery grid.
- Lock the structure, free the words: templates and patterns lock their layout, while text, images and links stay editable for Benjamin.
- URLs follow the page map exactly. Hubs and guides are hierarchical pages (/buying-property-in-cyprus/costs/). Post types: project at /new-developments/{slug}/, case at /cases/{slug}/. Child pages such as /new-developments/apartments-paphos/ must still resolve, so handle that rewrite conflict and test it.
- Linguise serves Dutch at /nl/. Keep every visible string in HTML (no text in images or CSS content), put the language switcher in header and footer, and exclude brand names (Cyprus Property Advisor, Cyprus-Consult) from translation.
- Rank Math owns titles, meta descriptions, breadcrumbs, sitemaps and redirects. The plugin adds schema only for types Rank Math cannot produce, and never outputs the same schema twice.
- Staging is noindex and password protected. Production becomes indexable on launch day only.

## Release criteria
- Mobile, measured on staging with PageSpeed Insights: LCP under 2.5 s, CLS under 0.1, INP under 200 ms.
- At most four font files, only the hero font preloaded; responsive images (srcset, AVIF or WebP); lazy loading below the fold; no jQuery in the theme; JavaScript only for navigation, intake form, calculators and table of contents.
- WCAG 2.2 AA: keyboard navigation, visible focus, labelled fields, reduced motion respected.

## Leads and integrations
- Intake form (M01), multi-step: saved in WordPress as a private lead post, emailed to Benjamin in a fixed format (name, email, phone or WhatsApp, country, segment, budget, area, timeline, financing, language EN or NL, landing page, UTM source, medium and campaign, referrer), then on to TidyCal booking (name and email prefilled if TidyCal supports it), and added to Kit with a segment tag.
- Consent checkbox with privacy link, Turnstile, no personal data in URLs or analytics, and a retention period that matches the privacy policy.
- Every other form uses the same pipeline.
- GA4 events from seo-spec.md (match call booked, inspection trip requested, guide download, webinar, calculator result emailed, course bought, done-for-you requested), fired only after consent.
- Calculators (purchase costs, rental yield, capital gains tax): vanilla JS blocks. Every rate lives on a settings page Benjamin can edit, shown with its year and a disclaimer.

## Content pipeline
- A script turns /content/*.md into WordPress drafts with the right parent, slug, template and pattern sections, plus the Rank Math title, description and focus keyword (register those fields for the REST API in the site plugin). Authenticate with an application password from .env.
- Never publish. Benjamin reviews and publishes.

## Deploy and safety
- One branch per feature, small commits, merge to main after Benjamin approves on staging.
- main deploys to staging (Cloudways Git deployment or a GitHub Action). Production deploys only after Benjamin's explicit "go".
- Never change the production database or settings without explicit approval. Take a Cloudways backup before every production deploy and every plugin update.
- Redirects from cyprus-consult.com are set on that site, not here. Deliver them as a list or a Cloudflare bulk-redirect file, only for pages that are live.
- After each task: a plain-language summary, what to check on staging, and a CHANGELOG entry.

## Handover
Keep /docs/runbook.md current: updating plugins safely, restoring a backup, adding a page from a pattern, adding an example project, changing calculator rates, and which changes need Claude Code.
