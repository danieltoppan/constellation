# Project context

Paste this into the Claude Project's Context field, and upload `CSI_Acquisition_Criteria.pdf` alongside it.

---

ABOUT THIS PROJECT

Purpose: build a sourced shortlist of vertical market software (VMS) companies that Constellation Software (CSI) could acquire, and write a brief thesis on the best ones. This is for a CSI business development interview, so every output must hold up when an M&A professional questions it.

HOW THE ENGINE WORKS

1. Sourcing (done by me, outside Claude)
I search Apollo with filters for industry (software), keywords for the vertical served, geography and headcount. I export the results as a CSV and attach it to a chat.

2. Screening (skill: vms-target-screener)
- Clean the CSV: dedupe, drop rows outside the headcount range.
- Pass 1: cut companies that are not software vendors, sell horizontal products, are customers in the vertical rather than vendors to it, or are already owned (by CSI or its groups, a rival acquirer, a PE firm or a strategic buyer). Keep cut rows with the reason.
- Pass 2: research the top survivors on the web and score each out of 100 against CSI's criteria (see the attached criteria PDF). Unsourced = unknown = 0 points.
- Output: a ranked CSV of all rows, plus a top-10 table in chat.
- STOP here. I pick which companies get a thesis.

3. Thesis (skill: vms-acquisition-thesis)
- One company at a time, only ones I pick.
- One page: snapshot, why it fits, why now, deal shape, risks, what could not be found, likely pushback with answers, first-call questions, how it was found, sources.

RULES FOR EVERYTHING
- Every fact links to a page actually opened. Search snippets are not sources.
- Unknown stays unknown. Never guess revenue, churn or ownership.
- Label estimates and show the method.
- Check for companies with the same name before using a source.
- Plain, brief language. No hype words. No em dashes.
