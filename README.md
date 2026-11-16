# VMS Deal Origination Engine

An AI-assisted workflow for finding, screening, writing up and approaching the owners of vertical market software (VMS) companies that a permanent-hold acquirer such as Constellation Software might buy. Built with Claude skills; every step keeps a human review gate.

## How it works

1. **Source (Apollo):** filter on industry = software, keywords for the vertical served, geography and headcount. Export a CSV.
2. **Screen (`vms-target-screener`):** clean and dedupe; cut non-vendors, horizontal products and anything already owned (CSI groups, rival acquirers, PE, strategics); research the top survivors and score each out of 100 against CSI's published criteria. Unsourced facts score zero.
3. **Review:** I pick which companies are worth a thesis.
4. **Write up (`vms-acquisition-thesis`):** a one-page, fully sourced thesis per company: fit, why now, deal shape, risks, open questions, likely pushback, first-call questions.
5. **Review:** I check the thesis facts and decide which owners to approach.
6. **Draft outreach (`owner-outreach-drafter`):** a short, soft first-touch email, call opener, follow-up note and contact cadence for each owner, built only from sourced facts in the thesis. No offers or pricing; the ask is a conversation. Drafts only: I edit and send every message myself.

## Principles

- Every fact links to a page that was actually opened.
- Unknown stays unknown; estimates show their method.
- Ownership is checked before anything else.
- AI does the legwork; a person approves every list, checks every fact and sends every message.
- Outreach is low volume and personal: one real reason to write, phone alongside email, no automated sequences.

## Repo layout

```
skills/
  vms-target-screener/SKILL.md     Screening and scoring skill
  vms-acquisition-thesis/SKILL.md  One-page thesis skill
  owner-outreach-drafter/SKILL.md  Owner outreach drafting skill
project/
  instructions.md                  Claude Project instructions
  context.md                       Claude Project context
  CSI_Acquisition_Criteria.pdf     Criteria reference
```

## Using the skills

Zip a skill's folder (for example `vms-target-screener/`) and upload it in Claude under Customize > Skills (or Plugins). Then create a Claude Project, paste in `project/instructions.md` and `project/context.md`, and upload the criteria PDF.

## Scoring rubric (100 points)

| Criterion | Points |
| --- | --- |
| Vertical focus | 15 |
| Mission-critical, high switching cost | 20 |
| Recurring revenue | 15 |
| Market position | 15 |
| Customer breadth | 10 |
| Longevity and stability | 10 |
| Seller readiness | 15 |

Criteria source: [Constellation Software acquisition criteria](https://www.csisoftware.com/?p=464).
