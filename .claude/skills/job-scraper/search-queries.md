# Search Queries for Job Scraper

## Installed portal CLIs (primary for `/scrape`)

`/scrape` discovers every portal skill under `.agents/skills/*/SKILL.md` and runs its CLI first. Shipped country-agnostic CLIs include `linkedin-search` and `freehire-search`; Danish demos and any skill you add with `/add-portal` are included the same way. You do **not** need a matching `site:` line below for those CLIs to run.

**Active for Adrian:** `linkedin-search` and `freehire-search` (both enabled by default). The four Danish portal demos (Jobindex, Jobbank, Jobdanmark, Jobnet) are **disabled** and should stay that way; they cover the wrong market. Romanian boards (eJobs, BestJobs, Hipo) have no CLI skill yet, so they run through the `site:` fallback below. If Romanian-local roles start mattering more than remote EU/US ones, scaffold one with `/add-portal`.

The `site:` query templates in this file are the **WebSearch fallback** — for portals without a CLI, company career pages, or when a CLI fails.

**Language scope:** Adrian's Languages table is Romanian (fluent) and English (fluent). Write query categories in **English first** — the remote EU/US market Adrian targets posts almost entirely in English — and add Romanian variants only for the local-market category. A posting requiring a language he has *not* declared, as a job condition, is excluded before scoring; a posting requiring a *higher level* than declared is flagged for his own judgment, not excluded — see `04-job-evaluation.md`'s Language Gate.

## Search Sites

Primary:
- **linkedin.com/jobs** - filter: Remote, Europe / United States; also covered by the `linkedin-search` CLI
- **freehire** - covered by the `freehire-search` CLI (contract/freelance oriented, matches Adrian's PFA setup)
- **wellfound.com** (formerly AngelList Talent) - startup and founding-engineer roles, remote filter
- **weworkremotely.com**, **remoteok.com**, **remotive.com** - remote-first boards
- **ycombinator.com/jobs** - early-stage, AI-heavy, often remote

Secondary (Romanian market, `site:` fallback only):
- **ejobs.ro**, **bestjobs.eu**, **hipo.ro**

Secondary (company career pages via Google `site:` filters).

## Query Categories

Queries are grouped by priority. Combine with remote filters rather than city filters — Adrian works fully remote from Romania for EU/US clients, so a city filter actively hurts recall.

### Priority 1: Senior / Staff engineer with AI at the center

Adrian's strongest and most desired direction.

```
site:linkedin.com/jobs "AI Engineer" remote Europe
site:linkedin.com/jobs "LLM Engineer" remote
site:linkedin.com/jobs "Senior AI Engineer" remote contract
site:linkedin.com/jobs "Staff Software Engineer" AI remote
site:wellfound.com "AI Engineer" remote
site:weworkremotely.com "AI" engineer senior
"LLM" "RAG" engineer remote contract Europe
"AI Engineer" "LangChain" OR "LangGraph" remote
```

### Priority 2: Founding / first engineer at AI-native startups

Matches the ownership pattern that shows up across his whole history.

```
site:wellfound.com "founding engineer" remote
site:ycombinator.com/jobs "founding engineer" remote
site:linkedin.com/jobs "Founding Engineer" remote
site:linkedin.com/jobs "first engineer" OR "founding engineer" AI
"founding full-stack engineer" AI remote
```

### Priority 3: Senior full-stack (React/Next.js + Node/Nest), remote contract

The reliable-volume category. Wider net, still well inside his core stack.

```
site:linkedin.com/jobs "Senior Full-Stack Engineer" remote Next.js
site:linkedin.com/jobs "Senior Software Engineer" React Node.js remote contract
site:remoteok.com senior full-stack typescript
site:remotive.com "full stack" senior typescript
site:weworkremotely.com "Next.js" OR "Nest.js" senior
"Senior TypeScript Engineer" remote contract Europe
```

### Priority 4: Domain-adjacent (fintech, legaltech, devtools)

Where his domain experience is a differentiator rather than neutral.

```
site:linkedin.com/jobs fintech "senior engineer" remote TypeScript
site:linkedin.com/jobs legaltech OR "legal tech" engineer remote
site:linkedin.com/jobs "developer tools" OR "devtools" senior engineer remote AI
"payments" OR "billing" "senior engineer" remote Stripe
```

### Priority 5: Romanian local market (lower priority, Romanian-language)

Only relevant if a local role is remote and clears the rate floor. Run these in Romanian.

```
site:ejobs.ro "inginer software senior" remote
site:bestjobs.eu programator senior remote TypeScript
site:hipo.ro "senior software engineer" remote
site:ejobs.ro "AI engineer" remote
```

## Location Filter

There is no commute tier. The only question is whether the work can be done entirely from Romania.

- **Ideal:** fully remote, worldwide or "work from anywhere"
- **Ideal:** fully remote, EU-wide (Romania included), or explicitly hires contractors in Romania
- **Acceptable:** fully remote US company hiring international contractors, with EET-workable overlap (US East Coast afternoons work; sustained US Pacific hours is a FLAG)
- **Reject:** hybrid, or any stated on-site days
- **Reject:** remote but geographically restricted in a way that excludes Romania ("US residents only", "must reside in Germany", in-country work authorization required)
- **Reject:** anything requiring relocation

## Rate Filter

**Hard floor: EUR 35/hour** (or the salaried equivalent). Drop any posting that states compensation clearly below it. Postings with no stated compensation still get scraped, but flag the rate as unverified so it gets raised early rather than after three interview rounds.

## Language Filter

Working languages: Romanian (fluent), English (fluent). Apply `04-job-evaluation.md`'s Language Gate: a posting requiring a language not on that table is excluded; a posting requiring a higher level in a declared language is flagged, not excluded. Postings merely *written* in another language, for a role that only needs English on the job, are fine.

## Date Filter

Only include jobs posted within the last 14 days, or with an application deadline that has not yet passed. If a posting date cannot be determined, include it but flag as "date unknown".

## Adapting Queries

If the user specifies a focus area, select queries from the matching category and also generate 2-3 custom queries for that focus. For example:
- `/scrape ai` -> Priority 1 and 2 queries plus focus-specific variants
- `/scrape contract` -> freelance/contract-weighted queries across all priorities
