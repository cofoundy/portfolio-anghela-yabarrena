# QA Report: Anghela Yabarrena Bravo

**Date:** 2026-02-11
**URL:** https://cofoundy.github.io/portfolio-anghela-yabarrena/
**Status:** FAIL

## Data Validation
- [x] Name matches source (Sheet: "Anghela Yabarrena Bravo", CV: "ANGHELA YABARRENA BRAVO", Page: "Anghela Yabarrena Bravo")
- [x] Email matches source (Sheet + CV + Page: anghelayabarrena@gmail.com)
- [x] Job title from source (CV: "Medico General", "Medico Emergencia" -> Page: "Medico General | Emergencias")
- [x] Companies listed exist in CV (Hospital Sabogal, PADOMI, Guillermo Kaelin, Hospital Solidaridad Surquillo, MINSA Paucarcolla -- all confirmed in CV)
- [x] Education institutions exist in CV (AHA, OPS, UPCH -- all confirmed)
- [x] Dates match source (all 6 experience date ranges match CV exactly)
- [~] "Telemedicina" skill listed but not explicit in CV. Inferred from Linea 113/SANNA roles. Borderline -- not flagged as hallucination.
- [x] No hallucinated data detected

## Clean Deploy
- [!] **FAIL** -- Programming symbols pattern in hero background (`</>`, `=>`, `[]`, `()`, `++`, `;`) -- developer-oriented template artifact completely inappropriate for a medical professional
- [!] **FAIL** -- Footer renders LinkedIn, Twitter, and GitHub social icons as `<a>` tags without `href` attribute (empty links). Client has no LinkedIn/Twitter/GitHub. These are dead template links that should be conditionally rendered.
- [x] No "Powered by" / "Made with" / "Built with" visible text
- [x] No "Lorem ipsum" / placeholder text
- [x] No "undefined" or "null" visible in content
- [x] No Astro logo or Vercel badge visible

## Technical
- [x] Page loads (HTTP 200)
- [x] CSS loads (HTTP 200 -- `/_astro/index.z47Fn8Iq.css`)
- [x] Favicon loads (HTTP 200 -- `/favicon.svg` with "AY" initials in #1e40af)
- [x] No profile image expected (client marked "Sin foto", hero has no `<img>` tag)
- [x] `astro.config.mjs` has correct `site` + `base` configuration
- [x] `<html lang="es">` set correctly
- [x] Google Fonts (IBM Plex Mono) loads via preconnect
- [ ] Console errors: Could not verify (Chrome MCP server unavailable)
- [ ] Screenshots: Could not capture (Chrome MCP server unavailable)

## Issues Found

### Issue 1: Programming Symbols in Hero (CRITICAL)
**Location:** Hero section SVG pattern (`#programming-symbols`)
**Problem:** The hero background contains a decorative SVG pattern with programming/code symbols: `</>`, `=>`, `[]`, `<>`, `()`, `::`, `==`, `++`, `;`. These are developer symbols from the `minimal-mono` template that are completely out of place for a **medical professional**. A doctor's portfolio should not have code syntax as background decoration.
**Fix:** Either remove the `#programming-symbols` pattern entirely, or replace with medical-themed symbols (caduceus, heartbeat, stethoscope, etc.), or use a neutral geometric pattern.
**File:** `src/components/Hero.astro` -- remove/replace the `<pattern id="programming-symbols">` block.

### Issue 2: Dead Social Links in Footer (CRITICAL)
**Location:** Footer section
**Problem:** The footer renders four social link icons (Email, LinkedIn, Twitter, GitHub). Only Email has a valid `href="mailto:anghelayabarrena@gmail.com"`. The LinkedIn, Twitter, and GitHub `<a>` tags have NO `href` attribute, making them dead/broken links. Clicking them leads nowhere.
The client's `config.ts` only defines `social: { email: "..." }` -- no linkedin, twitter, or github. The Footer component does not conditionally render these icons.
**Fix:** Update `Footer.astro` to conditionally render social icons only when the corresponding URL exists in `siteConfig.social`. Use optional chaining: `{siteConfig.social?.linkedin && (<a href={siteConfig.social.linkedin}>...</a>)}`.
**File:** `src/components/Footer.astro`

### Issue 3: Missing Accent on "Educacion" (MINOR)
**Location:** Education section heading and nav links
**Problem:** Section heading reads "Educacion" instead of "Educacion" (the Spanish word requires an accent: "Educacion"). Same issue in nav header and footer nav.
**Fix:** Update the section label in the component to use proper Spanish: "Educacion".
**Note:** This appears to be a template-level label issue, not config-related.

## Summary of Verdict

Two critical issues prevent a PASS:
1. The programming symbols in the hero background make this look like a developer portfolio, not a medical professional's site. This is a template artifact.
2. Three dead social links (LinkedIn, Twitter, GitHub) in the footer render as clickable icons that go nowhere. This is a template conditional rendering bug.

## Evidence
- Screenshots could not be captured (Chrome MCP server unavailable during QA session)
- Full HTML source downloaded and analyzed at /tmp/anghela-page.html
- All HTTP checks performed via curl
