# Where 40 Analyst Hours Go: A Matched-Resource Approach to Australian Charity Giving
> Analytics skills challenge reframing 22,506 Australian charities as a matched-resource problem — joining two ACNC datasets on ABN, scoring on a 4-pillar fit model, and recommending the top 10 where 40 weekly analyst hours would have the most impact.

**Stack:** Python · Pandas · NumPy · Tableau · PowerPoint
**Data:** [ACNC AIS 2020](https://data.gov.au/data/dataset/acnc-2020-annual-information-statement-data/resource/9eda5c10-bded-410e-8ee9-a327a8ff2560) · [ACNC Charity Register](https://data.gov.au/data/dataset/acnc-register/resource/eb1e6be4-5b13-4feb-b28e-388bf7c26f93)   
**Live Dashboard:** [Tableau Public](https://public.tableau.com/app/profile/talay.kamali/vizzes)

---

## TL;DR

A team of 20 analysts has 40 hours a week to give to a philanthropic cause. The brief asks which Australian charities they should support.

I rejected the obvious framing of "which charities need the most help" — that treats every form of help as interchangeable. I built it as a **matched-resource problem** instead: the team brings analytical capacity, not money, so the real question is where 40 hours a week of analytical work would actually have the most impact.

I joined two ACNC datasets on ABN (44,843 matches), filtered to 22,506 eligible charities, and scored each on a 4-pillar fit model. **Sensitivity check holds:** 8 of the top 10 remain top-ranked when you switch to equal weighting — the ranking isn't an artifact of the weights I chose.

**Recommendation:** contact the top 10. Lead with **Queensland & Northern NSW Lions Medical Research Foundation** (Fit Score 96.8) — the only charity in the shortlist to score in the top decile across all four pillars.

---

## The story behind it

The brief is the kind of problem that looks like a ranking exercise but isn't. ~50k Australian charities. 40 hours a week of analyst time to give. Most candidates will rank charities by impact or need and call it done. That's the wrong answer because **analyst hours aren't dollars** — a donation, a delivery shift, and a regression model are not interchangeable inputs.

A donation goes to whoever needs money. Analyst time goes to whoever needs analysis. The charities that benefit most from 40 weekly hours of analytical work are the ones where analytical capacity is the bottleneck — not the ones with the biggest general resource gap.

This project asks: **given that framing, who are the 10 charities the team should actually contact?**

---

## Project structure

```
Charity-Analytics/
├── notebooks/
│   └── charity_analysis.ipynb           # Full pipeline: load, join, filter, score, export
├── tableau/
│   └── Charity-Analytics.twbx           # 3-dashboard interactive workbook
├── deck/
│   └── charity_analytics_challenge.pptx # 12-slide presentation
├── outputs/
│   ├── charity_shortlist.csv            # 22,506 scored charities, top-10 flagged
│   └── figures/                         # Dashboard exports (PNG)
├── data/                                # (gitignored — see Reproducibility)
└── README.md
```

The notebook is self-contained — re-loads from raw Excel, applies the same join, filter funnel, and scoring, and produces results that reconcile with both the dashboard and the deck.

---

## Interactive dashboard

The Tableau workbook is published live on [Tableau Public](https://public.tableau.com/app/profile/talay.kamali/vizzes) — three connected dashboards in one workbook.

### Dashboards

1. **Sector Snapshot** — 4 KPI callouts (22,506 charities · 861K volunteers · $3.9B revenue · 77% Small), charity size distribution, cause-area coverage across 12 ACNC purposes, volunteers-vs-paid-staff scatter on log scale to show the leverage story
2. **The Shortlist** — top 10 ranked bar with Fit Score labels, 4-panel pillar breakdown small multiples (Fit · Impact · Capacity · Legitimacy), top 10 by state breakdown
3. **Charity Profile** — parameter-driven drill-down restricted to the top 10; pick any charity and see name, state, size, years operating, and the four pillar scores

Style is restrained: teal `#0D5C63` primary, amber `#D97706` for highlights. White background. Georgia headers, Calibri body. No gridlines, no 3D, no gradients. Editorial feel, not corporate-template.

---

## The 4-pillar fit model

Every eligible charity is scored 0–100 on four pillars, then combined into a weighted composite.

| Pillar | Weight | What it measures |
|---|---|---|
| **Impact** | 30% | Volunteer count, volunteer-to-paid-staff leverage, geographic spread, beneficiary count |
| **Cause** | 25% | Weighted toward acute need — homelessness, disability, Aboriginal/TSI, disaster victims get higher weights |
| **Legitimacy** | 25% | Positive net assets, low admin expense ratio, years of operation |
| **Capacity Gap** | 20% | Small/Medium size, low paid FTE, has a website — where analyst hours actually fit |

**Composite Fit Score** = 0.30 × Impact + 0.25 × Cause + 0.25 × Legitimacy + 0.20 × Capacity Gap

The weights reflect editorial judgment — the team should prioritize charities where analytical work moves outcomes, not just charities with the highest raw metrics. Sensitivity check below mitigates the "weights are arbitrary" concern.

---

## Key findings

### From the data join and filter funnel (notebook §1–4)

- **AIS 2020 has 51,392 charities; the Register has 65,629** — joined on ABN with a float-vs-int casting fix, 44,843 matched both
- **Filtered to 22,506 eligible** after dropping inactive, unregistered, zero-revenue, religion-only, and Large charities
- **77% of the eligible pool is Small** (vs 65% of the unfiltered sector — clarified in the deck footer to pre-empt the question)
- **Sector revenue is concentrated:** the top 4% of charities hold 80% of total sector revenue; everyone else shares the remaining 20%

### From the scoring model (notebook §5–7)

- **Top 10 Fit Scores span 82.3 to 96.8** — meaningful separation from the rest of the pool
- **NSW dominates the shortlist** (5 of 10), followed by QLD (3) and VIC (2)
- **Cause mix is diverse:** medical research, burns/health, surf life saving, environmental, disability services, aged care, community care, wellness foundations — exactly the breadth the team should engage with
- **#1 is the only top-10 charity in the top decile on all four pillars** — Queensland & Northern NSW Lions Medical Research Foundation: 1,000 volunteers, 45 years of operation, positive net assets, ~$156K annual revenue, near-zero paid FTE

### From the sensitivity check (notebook §7)

- **8 of the top 10 hold under equal weighting** — the ranking is robust to weight choice
- This kills the "the weights drive the result" critique before it gets asked

---

## What 40 hours actually looks like

The shortlist is the **who**. The deck closes with the **what** — analytical work matched to what each charity's data shows they do, not generic pro-bono "help":

- **Medical research** → grant ROI dashboard, donor cohort retention model
- **Health / Burns Association** → patient outcome tracking, peer-benchmark visualization
- **Surf Life Saving clubs** → volunteer-hour productivity, incident heatmaps by beach
- **Environmental** → site-prioritization model, campaign reach attribution
- **Disability services** → participant journey analytics, event ROI by region
- **Aged & children charities** → service-gap mapping, demand forecasting
- **Knit4Charities** → volunteer matching, output-distribution optimization

Each deliverable is reproducible from the AIS data feed. No new data collection required to start.

---

## Methodology highlights

What separates this from a standard "rank the charities" submission:

1. **Reframed the problem before scoring** — matched-resource instead of generic need-ranking. Easier to defend in interview because the framing earns the rest of the choices
2. **Two datasets, not one** — joined ACNC AIS 2020 on ACNC Charity Register on ABN. The register adds mission and beneficiary data the AIS doesn't have
3. **Defensible filter funnel** — every exclusion (inactive, unregistered, zero-revenue, religion-only, Large) is justified by the framing, not by convenience
4. **Min-max scaling with 99th percentile clipping** — kills outlier dominance in the pillar scores without losing the ranking signal
5. **Sensitivity check on weights** — equal-weight comparison shows 8 of 10 hold. The result isn't an artifact of my chosen weights
6. **Cause weighting is editorial and labeled as such** — acute-need bias is a design choice, not a data signal. Saying that out loud is more defensible than pretending it's neutral
7. **Restricted the Tableau parameter to top 10 only** — drill-down dashboard ships clean instead of letting users pick charities with missing fields

---

## Limitations and honest caveats

- **Self-reported data** — AIS figures come from the charities themselves; ACNC audits on cycle, not in real time
- **2020 financials** — the AIS dataset is five years old. Some top-10 charities may have changed substantially since then; first contact would verify current state
- **Cause weighting is editorial, not data-driven** — acute-need bias reflects judgment. The sensitivity check covers this but doesn't eliminate it
- **No qualitative signal** — the model doesn't measure leadership quality, organizational culture, or willingness to accept external help. That's a Step 2 problem, surfaced through outreach not through scoring
- **State field is missing for ~9% of the broader pool** — not an issue for the top 10 (all have complete records), but the parameter-driven dashboard is restricted to the top 10 partly for this reason
- **Alteryx was substituted with Python** — the brief explicitly allowed this with documented methodology; the deck scopes the equivalent Alteryx pipeline (Input → Join on ABN → Formula tools per pillar → Score → Filter Top 10 → Output) on slide 5

---

## Reproducibility

```bash
# Clone the repo
git clone https://github.com/talaygh/Charity-Analytics
cd Charity-Analytics

# Install dependencies
pip install -r requirements.txt

# Download the datasets (both data.gov.au, free, no signup)
# AIS 2020:        datadotgov_ais20.xlsx
# Charity Register: datadotgov_main.xlsx
# Place both in ./data/

# Run the notebook
jupyter notebook notebooks/charity_analysis.ipynb
```

The notebook uses no random sampling — the whole eligible pool of 22,506 charities is scored every run, so results are deterministic.

---

**Author:** Talay Kamali ·

## Connect

[LinkedIn](https://www.linkedin.com/in/talaykamali) · [GitHub](https://github.com/talaygh) · [Tableau Public](https://public.tableau.com/app/profile/talay.kamali/vizzes) · [Email](mailto:talayapps@gmail.com)

If you found this useful — or have feedback on how I'd improve it — I'd love to hear from you.
