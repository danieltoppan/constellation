---
name: vms-target-screener
description: Screen, research, score and rank a CSV of software companies against Constellation Software style VMS acquisition criteria. Use when given a company list to qualify.
---

# VMS Target Screener

Turns a raw company list (usually an Apollo, Sales Navigator or directory export) into a ranked, sourced shortlist of vertical market software (VMS) acquisition targets. It does NOT write theses; hand the shortlist to `vms-acquisition-thesis`.

## Inputs

- A CSV (or pasted table). Expected columns, any naming: company name, website/domain, industry, keywords or description, headcount, location, founded year, revenue (optional), LinkedIn URL.
- Optional settings from the user. If not given, use these defaults and say so at the top of the output:
  - Geography: whatever the user named, else no filter
  - Headcount range: 10 to 500
  - Deep-research count: top 20 survivors
  - Verticals of interest: whatever the user named, else any

If a needed column is missing, continue with what exists and note the gap. Never ask more than 2 questions before starting.

## Core rules

1. Evidence or unknown. Every score must point to a source URL you actually opened. No source = "unknown" = 0 points for that criterion. Never fill gaps from memory or plausibility.
2. Revenue from list tools (Apollo, ZoomInfo) is a modeled guess. Never use it to disqualify. Record it as "vendor estimate" only.
3. Ownership is the most important filter. A great business that is already owned is not a lead.
4. Name collisions are common. Confirm the domain, HQ and product match before using any source. If a database page mixes two companies with the same name, say so and discard it.
5. Work in batches of 5 companies for web research, and print progress after each batch so nothing is lost if the run stops.
6. Use code to load, clean and dedupe the CSV and to compute scores. Do the judgment (reading sources) yourself.

## Procedure

### Step 0: Load and clean
- Parse the CSV with code. Normalize column names. Dedupe on domain, then on name.
- Drop rows outside the headcount range (keep rows with blank headcount, flag them).
- Apply the geography filter if given.
- Report: rows in, rows after cleaning.

### Step 1: Hard disqualifiers (Pass 1)

Pass 1a, from CSV fields only (no web): mark DISQUALIFIED if clearly any of:
- Not a software vendor: agency, consultancy, IT services, reseller/VAR, staffing, hardware maker
- The company is a customer in the vertical, not a vendor to it (for example a propane dealer instead of propane software)
- Horizontal product sold to every industry (generic CRM, generic HR, generic accounting)
- Consumer app

Pass 1b, quick web check on survivors (one or two searches each: "<company> acquired", "<company> backed by" or "portfolio company"):
- DISQUALIFY if owned by or merged into: a Constellation group (Volaris, Harris, Jonas, Vela, Perseus, Topicus, Lumine, Total Specific Solutions, or any CSI subsidiary: note as "already CSI"), a rival serial acquirer (Valsoft, Aspire, Banyan, Tiny, Upland, Roper, Vista, Thoma Bravo, and similar), a strategic buyer, or a PE firm.
- FLAG (do not disqualify) if VC-backed: may still sell, but note it.
- DISQUALIFY if defunct or the website is dead.

Keep every disqualified row in the output with the reason and source. The cuts prove the filter works.

### Step 2: Deep research (Pass 2) on the top survivors

Rank survivors by a quick prior (vertical fit from description, years in business, headcount in range) and research the top N (default 20). For each, open at minimum: the company's own site (product, about, customers pages), one independent source (trade press, review site like Capterra or G2, or a press release), and an ownership check. Capture:

- What the product does in one plain sentence
- Vertical served (the customer industry) and geography
- Founded year, founder name, current leader, and whether they are the same person or family
- Headcount (source) and customer count or scale claims (source)
- Pricing model signals: subscription, SaaS, maintenance, per-seat
- Market position claims (#1, "leading", share figures) and who the rivals are
- Ownership status and evidence
- Trigger events in the last 24 months: leadership change, retirement, founder starting something new, anniversary, product shift, rival acquired

### Step 3: Score (100 points)

| Criterion | Points | Full marks when | Evidence |
| --- | --- | --- | --- |
| Vertical focus | 15 | Product built for one industry or a few close ones | Product pages, customer list |
| Mission-critical, high switching cost | 20 | Runs core operations: records, scheduling, dispatch, billing, compliance | Feature pages, migration claims |
| Recurring revenue | 15 | Subscription, SaaS or annual maintenance clearly stated | Pricing page, review sites |
| Market position | 15 | Credible #1 or #2 in its niche, or named by trade press as a main option | Third-party source preferred |
| Customer breadth | 10 | Hundreds or thousands of customers, no single giant customer | Customer counts, case studies |
| Longevity and stability | 10 | 15+ years in the same niche | Founded year, history page |
| Seller readiness | 15 | Founder 20+ years, generational handoff, leadership change, founder attention elsewhere, no outside capital | Press, LinkedIn, leadership pages |

Scoring rules:
- Partial credit is allowed; write one line of reasoning per criterion.
- Unknown = 0 and is listed as unknown, never guessed.
- Compute Evidence coverage = criteria with a source / 7. A company scoring 70 with 40% coverage is ranked BELOW a company scoring 65 with 90% coverage. Rank by score, then break ties and near-ties (within 5 points) by coverage.
- Revenue estimate, if useful: headcount x $150K to $250K per employee, labelled "rough estimate, method: headcount". Never score on it.

### Step 4: Output

1. A short header: settings used, rows in, rows after cleaning, disqualified count by reason, researched count.
2. Ranked shortlist table (top 10 in chat): Rank, Company, Vertical served, Score, Coverage, Trigger, One-line why, Ownership status.
3. A downloadable CSV (or the user's preferred sheet format) with ALL rows and columns: rank, company, domain, vertical_served, hq, founded, leader, headcount, headcount_source, ownership_status, ownership_source, disqualified (Y/N), disqualify_reason, score, coverage, score_breakdown, trigger, trigger_source, key_sources, unknowns, next_step.
4. A "Watch list" of near misses (for example VC-backed or recently bought by an owner-operator who might sell later).
5. Recommend which 3 to 5 to send to `vms-acquisition-thesis`, and why.

## Quality checklist before finishing

- Every non-zero score has a URL you opened
- Ownership checked for every researched company
- No company appears twice
- Name collisions checked
- Disqualified rows kept with reasons
- Defaults and assumptions stated at the top
