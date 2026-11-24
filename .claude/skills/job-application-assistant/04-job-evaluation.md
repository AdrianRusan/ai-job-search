---
framework_version: 1.2.2
---

# Job Evaluation Framework

<!-- SETUP: Skill match areas and career goals are personalized by running /setup -->

## Eligibility Gate — run before scoring

If the candidate is not a citizen or permanent resident of the country they are applying in, run this first. It is a hard filter, not a scoring dimension, and it is separate from work-permit *timing*: timing asks "can they work the required hours yet?", eligibility asks "are they permitted to hold this job at all?". A candidate can pass timing and still be categorically excluded.

Read the posting's eligibility / work rights / "who can apply" section **verbatim** and classify:

| Posting wording | Verdict |
|-----------------|---------|
| Names a **citizenship or permanent-residency requirement** ("must be a citizen of X", "permanent resident", "PR required", "full working rights" where the employer means citizen/PR) | **FAIL — hard stop.** Do not score, do not draft. Quote the exact wording back to the user. |
| Requires a **security clearance** at any level | **FAIL** in most countries, since clearance is normally gated on citizenship. Verify the specific scheme rather than assuming. |
| **Explicitly names** the candidate's permit class, or says "international applicants welcome", "visa holders considered", "we sponsor" | **PASS** — verified acceptance. Worth noting as a positive in the application. |
| **Silent** on citizenship or residency | **PROCEED, but mark unverified.** Check the employer's own careers or international-applicant page before drafting. |

**Two rules that are easy to get wrong:**

1. **Silence is not permission.** Large graduate programs frequently gate eligibility on their own website rather than in the job ad. Highest-risk categories: professional-services firms, government and defence, banking, telecommunications, and anything touching critical infrastructure.
2. **A company-wide "we accept international applicants" statement is not role-level permission.** The common pattern is a general welcome followed by a *named list* of the specific programs or service lines it covers. Confirm the **specific posting or stream** appears on that list before drafting.

**Report an eligibility failure to the user with the quoted source** rather than silently dropping the role. They may know something about their own status that the profile does not record.

If the candidate's permit also constrains *hours* or *start date* (a student visa with a term-time cap, a permit that begins on graduation), record that as a second gate under this section during `/setup`, with the specific dates. Do not merge it with the eligibility question above — they fail for different reasons and need different answers.

A role that fails this gate is not scored and not drafted. Everything below applies only to roles that pass it.

## Language Gate — run before scoring

No dimension or gate anywhere in this framework currently checks a posting's language requirements against what the candidate actually speaks - it is not one of the five Scoring Dimensions below, not a field `/scrape` or `/rank` track, and not something `/apply`'s language detection (Step 1, which already extracts a posting's required language generically) has anywhere to report to. This gate adds that check, structured the same way as the Eligibility Gate above: read the posting, classify against profile data, and treat a hard mismatch as FAIL before scoring.

Read the posting's language requirements as stated for **the role itself** — not the language the ad happens to be written in. A posting written in a language you don't work in, for a role that only needs languages you do work in on the job, passes fine; only an explicit job-condition requirement ("fluent X required," "must communicate with the Y team in Z") triggers this check. For each language the posting requires as a job condition, compare it against your Languages table in CLAUDE.md / `01-candidate-profile.md`:

| Posting requirement vs. your Languages table | Verdict |
|---|---|
| Requires a language **not on your table at all** (e.g. "fluent Polish required," "must communicate with the Warsaw team in Russian," and you list no Polish/Russian row) | **FAIL — hard stop.** Do not score, do not draft. Quote the exact requirement line. |
| Requires a language you **do** list, but the posting's stated bar (as written — "fluent," "native," "C1+," "business-level") reads as plausibly **higher** than your declared level | **FLAG, then proceed.** Not a fail. Score and draft normally, but surface the gap explicitly in your report to the user (quote both the posting's requirement and your declared level) so they can judge it themselves — bars like "fluent" vary a lot by company and geography, and a recruiter may be flexible. Never silently drop the posting and never silently treat it as a clean pass. |
| Requires a language you list, at or below your declared level (or the posting doesn't specify a level at all — just names the language) | **PASS.** No note needed. |

Judge the level comparison the same way you judge everything else in this framework: read both sides as written and reason about it, don't force either into a rigid scale — CEFR letters, LinkedIn-style buckets ("professional working proficiency"), and plain-English words ("conversational," "fluent," "native") all appear in the wild and don't map onto each other precisely. When genuinely unsure whether a stated bar exceeds the candidate's level, prefer FLAG over a silent PASS — the human is meant to be the tiebreaker, not the gate.

**Worked example:** a candidate whose Languages table lists Spanish (Native) and English (B1/B2). A posting requiring "fluent Russian" → **FAIL**, Russian isn't declared at all. A posting requiring "fluent English" → **FLAG**, English is declared but "fluent" plausibly exceeds B1/B2 — score and draft the application, but tell the candidate this posting's bar may be a stretch and let them decide. A posting requiring "conversational English" or unspecified English → **PASS**, B1/B2 clears a "conversational" bar cleanly.

## Scoring Dimensions

Evaluate each job posting against these five dimensions:

### 1. Technical Skills Match (0-100)
How well do the required/preferred skills align with the candidate's capabilities?

| Score | Meaning |
|-------|---------|
| 80-100 | Core requirements are primary skills |
| 60-79 | Most requirements match, 1-2 gaps that are learnable |
| 40-59 | Partial match, significant upskilling needed |
| 0-39 | Fundamental mismatch |

**Strong match areas:** React, Next.js, TypeScript, Node.js, Nest.js; LLM pipeline engineering (LangChain, LangGraph, OpenAI API, Claude API, RAG, automated evals, cost-optimized model routing); PostgreSQL/Prisma; full-stack product delivery end to end; third-party and government API integration; Stripe/payments; agentic coding with Claude Code.

**Moderate match areas:** Python, FastAPI, GraphQL, Docker, MongoDB, MySQL, Redis, Supabase, CI/CD (GitHub Actions), Playwright E2E testing, N8N automation, OCR (Google Cloud Vision), Vercel/Railway deployment.

**Weak match areas:** Kubernetes and large-scale cloud infrastructure (AWS/GCP/Azure at platform depth); Go, Rust, Java, C#, PHP, Ruby; mobile (iOS/Android/React Native); training ML models from scratch (as opposed to orchestrating LLMs); classical data engineering (Spark, Airflow, dbt, warehousing); embedded/systems programming; formal security or compliance engineering.

### 2. Experience Match (0-100)
Does work history align with what they're looking for?

| Score | Meaning |
|-------|---------|
| 80-100 | Direct experience in the same domain and role type |
| 60-79 | Related experience, transferable skills clear |
| 40-59 | Adjacent experience, would need to make the case |
| 0-39 | Unrelated experience |

**Strong:** Senior/staff full-stack engineering on production web platforms (10+ years); founding-engineer and first-engineer roles; AI/LLM product engineering; fintech and government-integration products; legaltech workflows; SaaS from zero to production; freelance/contract delivery to external clients.

**Moderate:** Esports and live-broadcast systems; market research tooling; technical leadership and mentoring; data parsing and analytics dashboards; developer-experience and CI/CD improvement work.

**Entry-level / would need to make the case:** Formal engineering management (headcount, performance reviews, hiring); platform/SRE and infrastructure-first roles; ML research or model-training roles; data engineering roles; roles requiring a completed degree as a hard credential; large-enterprise process-heavy environments (all recent work is solo or first-engineer).

### 3. Behavioral/Culture Fit (0-100)
Does the role and company culture match the behavioral profile?

| Score | Meaning |
|-------|---------|
| 80-100 | Culture strongly matches behavioral preferences |
| 60-79 | Mixed signals but mostly compatible |
| 40-59 | Some friction areas |
| 0-39 | Significant culture mismatch |

**Red flags to research:** Department disorganization, work dominated by maintenance over development, poor chemistry with leadership, culture mismatches. Check reviews, media coverage, LinkedIn connections, and network contacts for insider perspective.

### 4. Location & Logistics (Pass/Fail + Notes)

Adrian works **fully remote from Brașov, Romania**, for EU or US clients. There is no commute tier
here: the question is only whether the role can be done entirely from Romania.

- Fully remote, EU or US employer/client, no office requirement: **PASS**
- Fully remote, worldwide / "work from anywhere": **PASS**
- Remote but restricted to a country or region that excludes Romania (e.g. "US-based only",
  "must reside in Germany", "EU work authorization in-country required"): **FAIL** - quote the
  restriction back to the user
- Hybrid, or any stated on-site days: **FAIL** (deal-breaker)
- Requires relocation: **FAIL** (deal-breaker)
- Remote with occasional travel for team offsites (a few times per year): **PASS**, note it
- Frequent international travel as a job duty: **FLAG** (discuss with user)
- Timezone overlap requirement: **PASS** if the required overlap is workable from EET (UTC+2/+3);
  **FLAG** if it demands sustained US Pacific hours or similar. Romania is EET, so most EU roles
  overlap cleanly and US East Coast roles are workable in the afternoon/evening.

### 4b. Rate / Compensation Floor (Pass/Fail)

**Hard floor: EUR 35/hour.** Anything below is a FAIL, not a negotiation.

- Posting states a rate or salary clearly at or above the floor: **PASS**
- Posting states a rate or salary clearly below the floor: **FAIL** - do not draft
- Posting gives no compensation information: **PROCEED, but flag as unverified.** Raise the rate
  early in the process rather than after investing in interviews.
- Salaried role rather than contract: convert to a comparable hourly figure before judging
  (roughly 1,700-1,800 billable hours/year is a reasonable divisor for a full-time equivalent),
  and account for the fact that Adrian invoices through his own PFA, so employer-side costs and
  benefits are not part of the number.

### 5. Career Alignment & Motivation (0-100)
Does this role advance career goals and contain tasks that energize?

| Score | Meaning |
|-------|---------|
| 80-100 | Strongly aligned with career direction, clear growth path |
| 60-79 | Good role but only partially aligned with long-term goals |
| 40-59 | Decent job but doesn't build toward career goals |
| 0-39 | Dead end or backwards step |

**Career goals:**
- Senior or staff-level engineering roles where AI/LLM work is central to the product, not a
  side experiment.
- Continue owning systems end to end: founding-engineer, first-engineer, or staff-level scope
  over narrow component ownership.
- Deepen LLM product engineering (pipelines, evals, guardrails, cost engineering, agentic systems)
  as the primary specialization rather than a secondary skill.
- Fully remote work for EU/US clients at or above the EUR 35/hour floor.

**Motivation filter:** Evaluate not just whether he *can* do the tasks, but whether they will
*energize* him.
- **Tasks that energize:** building AI/LLM pipelines end to end; greenfield and 0-to-1 product work;
  reverse-engineering undocumented systems and integrations; cost and reliability engineering on AI
  systems; shipping fast with real users on the other end; technical leadership and mentoring.
- **Tasks that drain:** maintenance-dominated work on legacy systems; narrow slice ownership inside
  a large process-heavy org; approval chains for routine technical decisions; roles where AI is
  decorative rather than load-bearing; presence/hours-based management.
- **Non-task factors:** degree of autonomy, directness of communication, distance between decision
  and production, whether the company genuinely operates async.

**Life situation alignment:**
- **Security:** freelancing through his own PFA with two concurrent clients. Not desperate for a
  role, which means marginal-fit postings should be scored honestly rather than generously.
- **Flexibility:** fully remote from Romania (EET). Contract/freelance structure preferred;
  full-time employment considered only if remote-from-Romania and above the rate floor.
- **Professional development:** wants depth in AI/LLM engineering and staff-level technical scope.
  Score a role higher when it puts him closer to the hard parts of an AI product.

### 6. Salary Benchmark (Optional)

If the salary lookup tool is configured (`salary_data.json` exists), look up the company:
```
python salary_lookup.py "<Company Name>" --json
```

If a city is known from the posting, add `--city "<City>"` to narrow results.

Present findings as:
```
### Salary Benchmark
| Metric | Value |
|--------|-------|
| [Category] index | XX.X (+/-X.X% vs baseline) |
| Overall index | XX.X (+/-X.X% vs baseline) |
```

Interpret results relative to the baseline defined in the data file's metadata. For index-based data, higher typically means above-market compensation.

If the salary tool is not configured, skip this section.

## Output Format

Present the evaluation as:

```
## Job Fit Evaluation: [Role] at [Company]

| Dimension | Score | Notes |
|-----------|-------|-------|
| Technical Skills | XX/100 | [brief note] |
| Experience Match | XX/100 | [brief note] |
| Behavioral Fit | XX/100 | [brief note] |
| Location | PASS/FAIL | [brief note] |
| Career Alignment | XX/100 | [brief note] |

**Overall Score: XX/100** (weighted average of scored dimensions)

### Verdict: [Strong Fit / Good Fit / Moderate Fit / Weak Fit / Poor Fit]

### Key Strengths for This Role
- [bullet points]

### Gaps to Address
- [bullet points]

### Recommendation
[1-2 sentences: apply/skip/apply with caveats]

### Company Research Checklist
- [ ] Checked company website (mission, values, recent news)
- [ ] Checked review sites (Glassdoor, Jobindex, etc.)
- [ ] Checked LinkedIn for team size, recent hires, connections
- [ ] Checked media for restructuring, growth, or workplace issues
- [ ] Identified network contacts who may know the team/manager
```

## Weighting
- Technical Skills: 30%
- Experience Match: 25%
- Behavioral Fit: 15%
- Career Alignment: 30%

(Location is pass/fail, not weighted)

## Thresholds
- **Strong Fit** (75+): Definitely apply, tailor everything
- **Good Fit** (60-74): Apply, address gaps in cover letter
- **Moderate Fit** (45-59): Consider carefully, discuss with user
- **Weak Fit** (30-44): Probably skip unless strategic reasons
- **Poor Fit** (<30): Skip

## Pre-Application: Call the Employer (Best Practice)

Before writing the application, consider whether the candidate should call the contact person listed in the posting. **Only call if there are substantive questions** - never call just to "be remembered."

### When to Suggest Calling
- The posting has unclear or ambiguous requirements
- It's unclear which competencies are essential vs. nice-to-have
- The role description is vague about day-to-day tasks
- There's a named contact person who invites questions

### Good Questions to Ask
- "What are the primary challenges in this role?"
- "How is time typically divided across the listed responsibilities?"
- "Which competencies are most critical for success in this position?"
- "What does success look like in the first 6-12 months?"

### Rules for the Call
- Prepare a 30-second "elevator pitch" about your background in case they ask
- The call's purpose is **gathering information**, not delivering a pitch
- Take notes - use what you learn to tailor the application
- Reference the conversation naturally in the cover letter ("After speaking with [name], I was especially drawn to...")
