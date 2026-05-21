# Claude Code prompts

## Why these exist

The Anthropic chat web fetcher refused to pull reqfit.com (system rejection, reason undetermined) during the original Arphie comparison work on 21/05/2026. This meant the initial analysis relied on stale assumptions from the reqfit-content-strategy skill which had not been refreshed since several recent product and content updates.

Claude Code running on Graeme's local machine has unrestricted network access and can fetch both reqfit.com and arphie.ai directly. Use these prompts to re-run the analysis with verified live data.

## Prompt 1: Full competitive analysis with live site access

Paste this into Claude Code in the reqfit-site repository (or any working directory):

```
You are running a competitive analysis between ReqFit (reqfit.com) and Arphie (arphie.ai). Both are AI tools for RFP and proposal work, but they target different segments. Your job is to fetch both sites, compare them systematically, and identify gaps in ReqFit's site that need to be addressed before launch.

CONTEXT YOU NEED TO KNOW

ReqFit positioning:
- Single use proposal review tool sold per report
- Target persona: sales managers and BD leaders at 30-80 employee firms
- Pricing: £49 / $49 per review with free first review (account required)
- Privacy-first architecture: stateless and logless processing, except for the requirements list which is retained against the user account so re-runs are consistent
- Self-serve, no demo required, no sales conversations needed
- AppExchange distribution channel as well as direct
- Operated by CASM Labs Ltd, UK company, ICO registered ZC111039

Arphie positioning:
- Multi seat enterprise platform for RFP response automation
- Gated pricing, demo-led sales motion
- General Catalyst backed, well funded, named enterprise customers
- SOC 2 Type 2 certified, Zero Data Retention claim
- Different segment entirely but will appear in same buyer searches

YOUR TASK

Step 1: Fetch the following ReqFit pages and capture the actual content. Use the WebFetch tool.

- https://reqfit.com/ (homepage)
- https://reqfit.com/how-it-works (process explanation)
- https://reqfit.com/pricing
- https://reqfit.com/about
- https://reqfit.com/faq
- https://reqfit.com/contact
- https://reqfit.com/blog
- Any /security page if it exists (it may not yet)

For each page, record:
- The actual on-page copy (especially hero, value props, CTAs)
- The page structure and section order
- Any trust signals visible (logos, badges, certifications, testimonials)
- Specific claims made about security, data handling, AI usage
- Navigation structure (top nav, footer)
- Forms and CTAs

Step 2: Fetch the following Arphie pages.

- https://www.arphie.ai/ (homepage)
- https://www.arphie.ai/security
- https://www.arphie.ai/features
- https://www.arphie.ai/about
- https://www.arphie.ai/integrations
- https://www.arphie.ai/sales
- https://www.arphie.ai/proposal

Record the same elements for each page.

Step 3: Produce a comparison document at ./arphie-vs-reqfit-comparison-LIVE.md with the following structure.

1. Executive summary: in 5 bullets, what are the biggest gaps in ReqFit's site relative to Arphie's, weighted by how a comparison shopper would experience them
2. Site architecture comparison: page-by-page table of what each has and what each lacks
3. Homepage messaging comparison: hero copy, value props, trust signals, CTAs side by side
4. Security and trust signal comparison: full audit of what each site claims about data handling, certifications, infrastructure, and where each claim lives on the site
5. Features comparison: where the two overlap, where they diverge, where ReqFit can credibly close the gap and where it should not bother
6. Pricing and commercial model: how each handles pricing transparency, free trial, sales motion
7. Content depth: blog, FAQs, glossary, industry guides, case studies
8. Where ReqFit looks weaker than Arphie (be precise and unflinching)
9. Where ReqFit looks stronger than Arphie (be specific)
10. Quick wins: pre-launch items that close real gaps without needing engineering, in priority order
11. Strategic gaps: post-launch items that need investment but matter for credibility

Step 4: Produce a second document at ./security-page-gap-analysis-LIVE.md focused specifically on the security gap.

This document should:
- Capture exactly what Arphie's /security page contains (each card, the hero, the FAQ section, any footer trust elements)
- Capture exactly what security messaging currently exists on reqfit.com and where it lives (homepage, how-it-works, FAQ, footer)
- Identify what ReqFit can credibly claim today that is NOT yet on a dedicated security page
- Identify what ReqFit cannot claim (SOC 2, ISO 27001, SSO) and how to handle that honestly
- Recommend a /security page structure for ReqFit that lifts Arphie's eight-card format but uses ReqFit's actual story

OUTPUT FORMAT REQUIREMENTS

- UK English spelling throughout (organisation, colour, analyse, recognise)
- Sentence case for all headings
- No em dashes anywhere, replace with commas, brackets, or split sentences
- Minimal hyphen use (avoid "well-known", "step-by-step", "out-of-the-box" style)
- Tables for structured comparisons
- Direct, action-oriented language
- Do not pad with caveats or apologies
- Cite specific copy from the live pages with quotes where relevant, so Graeme can verify

DO NOT

- Make claims about ReqFit based on the reqfit-content-strategy skill or any other internal documentation. Use only what is actually live on the site
- Suggest features ReqFit should add without considering whether they fit the per-review, self-serve, privacy-first positioning
- Recommend Arphie's feature set verbatim (different product, different segment)
- Treat SOC 2 as table stakes (it cannot be obtained in time for launch and is not the only credible security story)

When you are done, summarise in chat: the top 5 quick wins, the top 3 strategic gaps, and one paragraph on whether the security page work should still be the highest priority pre-launch item.
```

## Prompt 2: Refresh the reqfit-content-strategy skill with verified current state

Use this after Prompt 1 has produced an accurate site snapshot, to update the stale conclusions in the existing content strategy skill.

```
Read the file ./arphie-vs-reqfit-comparison-LIVE.md which captures the verified current state of reqfit.com as of today. Compare this against what is currently in /Users/graeme/skills/reqfit-content-strategy/ (or wherever that skill lives locally).

Identify every place in the reqfit-content-strategy skill where:

1. A statement about what is live on the site is now stale or incorrect
2. A statement about product capabilities is now stale or incorrect
3. The pricing differs from £49 / $49 per review
4. A reference file describes a feature or page that has since changed or shipped
5. The skill claims something is "missing" or "to do" that has since shipped

For each finding, produce a structured diff in a file called ./reqfit-content-strategy-refresh-LIVE.md showing:
- File: which file in the skill
- Section: which heading or block
- Stale text: what it currently says
- Verified text: what it should say based on the live site
- Confidence: how certain you are this needs changing

Do not edit the skill files themselves yet. Just produce the diff document so Graeme can review and apply changes deliberately. The skill is a single source of truth and applying changes without review risks losing important historical context.

UK English, sentence case headings, no em dashes, minimal hyphens.
```

## Prompt 3: Build the /security page in Astro

Use this when ready to actually build the security page. Reference the spec in security-page-spec.md in this skill.

```
Build a new /security page for reqfit.com in the existing Astro project.

The full page specification, including hero copy, all eight feature cards, the SOC 2 honest framing block, the 10 security FAQs, and the footer trust badges, is in the countdown-to-launch skill at references/security-page-spec.md. Read that file first.

Build requirements:

1. Use the existing component library
   - Card.astro for the eight feature cards
   - Badge.astro for trust badges in the footer block
   - Existing FAQ accordion pattern from /faq page for the FAQ section
   - BaseLayout.astro for the page wrapper with schema slot

2. JSON-LD schema
   - WebPage schema for page-level context
   - Organization schema with CASM Labs Ltd full contact, address, ICO registration
   - FAQPage schema for all 10 security FAQs visible on the page

3. Navigation updates
   - Add Security link to top nav between How it works and Pricing
   - Add Security column to footer with sub-links to /security, /privacy-policy, /responsible-disclosure (responsible disclosure page to be built separately)

4. Visual treatment
   - Match the visual treatment used on the how-it-works page for consistency
   - Use Deep Forest brand colour for trust accents
   - Eight cards in a 2x4 grid on desktop, single column on mobile
   - Hero with strong privacy-first headline

5. Performance
   - Same Lighthouse standards as existing pages (100% accessibility, 95+ performance on desktop)
   - All images optimised, no render-blocking external resources

6. Copy
   - UK English, no em dashes, sentence case headings
   - Use the exact card copy from the spec file. Do not paraphrase or "improve" it without flagging
   - Use the exact FAQ wording from the spec file

After building, list:
- Files created
- Files modified
- Any open questions about copy or visual treatment that need Graeme's decision before final styling

Do not deploy to production until Graeme has reviewed the build on a local server.
```

## Prompt 4: Build the Responsible Disclosure Policy page

Quick win, low effort. Use this for a brief Astro build.

```
Create a new /responsible-disclosure page for reqfit.com in the existing Astro project.

Page content: a clear, plain English Responsible Disclosure Policy for ReqFit (operated by CASM Labs Ltd, ICO registration ZC111039). Cover:

- Scope (what systems are in scope for responsible disclosure)
- How to report a vulnerability (contact security@reqfit.com)
- What ReqFit commits to (acknowledge within X business days, work on a fix, credit the researcher if they want recognition)
- Safe harbour (no legal action against good-faith researchers acting within the policy)
- Out of scope items (automated scanning without consent, denial of service testing, social engineering, anything that risks customer data)
- Public hall of fame (optional, for future)

Use a standard reasonable Responsible Disclosure Policy template adapted to ReqFit's actual size, current capabilities, and CASM Labs Ltd as legal entity. Plain English, no legal jargon where it can be avoided.

Build requirements:

1. Use BaseLayout.astro
2. Lift visual treatment from existing legal pages (privacy policy, terms)
3. Add to footer in Legal column
4. JSON-LD: WebPage schema only

UK English, sentence case, no em dashes.

List files created and modified after the build.
```

## Notes on running these prompts

- Run from inside the reqfit-site repository directory on the local machine so Claude Code has correct file paths
- Prompts 1, 3 and 4 can be run independently
- Prompt 2 depends on Prompt 1 having produced ./arphie-vs-reqfit-comparison-LIVE.md
- The output files (LIVE.md suffixed) are working documents, not skill content. Move accepted changes into the relevant skill manually after review
- If Claude Code's WebFetch returns rate limiting or blocking errors for reqfit.com (unlikely but possible from local network), the prompts can be adapted to use curl directly or to read static HTML exports
