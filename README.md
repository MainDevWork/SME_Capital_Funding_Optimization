**I built a data workflow that sorts 1,100+ SME funding applications into three funding-readiness tiers, and set out a recommended strategy for each tier to increase the number of businesses connected with investors.**

**Note**: The original dataset has been sanitized. Read the details by the [**About the data section**](#about-the-data)

---

## Summary

The SME Capital Matching team was not fully satisfied with the results achieved on the 2023 and 2024 SME campaigns. I took the 1,000+ applications received from 2025 onwards and built a funding-readiness segmentation data workflow that scores every applicant and places them into tiers, identifying the SMEs most ready to receive capital funding and be introduced to investors. I then provided recommended strategies for each tier, setting out how the businesses in that tier can be connected with investors in order to improve the overall conversion of SMEs into funded businesses.

## Background and problem statement

The JSE runs a programme called [SME Capital Matching](https://www.jse.co.za/services/sme-rise/sme-capital-matching-initiative). Small and medium enterprises apply for funding, and the strongest candidates are introduced to prospective investors. At the time of this work, more than a thousand applications had been received and progress had stalled.

The SME team within Marketing & Corporate Affairs had raised two concerns: the conversion rate, meaning the proportion of applicants who reached an investor, and the turnaround time required to get them there. Neither could be explained by a shortage of applicants or by weak investor appetite. The constraint was that the team had no means of determining, quickly and defensibly, which applicants merited presentation.

I identified three obstacles:

- **The applicant pool was larger than the team could assess manually.** Reviewing more than a thousand applications by hand is slow, and the outcome varies depending on who conducts the review.
- **The data was not usable as captured.** Applications were submitted through a web form, which introduced substantial quality problems: funding amounts padded with underscore characters (`250000_________`), revenue recorded as text bands rather than values, provinces recorded under four different spellings, duplicate submissions, and blank responses in fields that investors require.
- **There was no shared definition of funding readiness.** Without an agreed standard, each shortlist reflected individual judgement, and businesses were being presented to investors whose mandates they did not meet.

I identified this gap independently and developed the tool to address it.

## Deliverables

| File | Description |
|---|---|
| [Python_Notebooks/Data_Cleaning.ipynb](Python_Notebooks/Data_Cleaning.ipynb) | The data cleaning workflow, documented step by step |
| [Python_Notebooks/Funding_Readiness_Segmentation.ipynb](Python_Notebooks/Funding_Readiness_Segmentation.ipynb) | The scoring model and tier segmentation |
| [Datasets/Capital_Matching_Cleaned_Data.xlsx](Datasets/) | The cleaned dataset |
| [Datasets/Funding_Readiness_Segmentation.xlsx](Datasets/) | Every SME scored, tiered and ranked |
| [Funding_Readiness_Methodology.pdf](Funding_Readiness_Methodology.pdf) | How the readiness score is constructed, written for a non-technical reader |
| [SME_Funding_Optimization_Strategy.pdf](SME_Funding_Optimization_Strategy.pdf) | The recommended action for each tier, covering investor matching and a twelve-month conversion plan |
| [SME_Capital_Matching_Dashboard.pbix](SME_Capital_Matching_Dashboard.pbix) | Power BI dashboard built on the segmented data |

## The solution

I built a repeatable workflow that scores every applicant on funding readiness and assigns each one to a tier. This replaces discretionary shortlisting with a consistent and auditable basis for prioritisation, which allows the team to move from sifting applications to matching businesses with investors.

Each business receives a score between 0 and 100, calculated against four weighted pillars drawn from fields the application form already collects. The score determines the tier:

| Tier | Score | Definition | Recommended action |
|---|---|---|---|
| **High** | 62–100 | Established, credible businesses submitting appropriately sized funding requests | Package and present to investors immediately |
| **Mid** | 48–61 | Genuine operating businesses with addressable gaps | Develop: provide readiness support, right-size the funding request, and re-score |
| **Low** | 0–47 | Early-stage businesses, or applications with material information gaps | Remediate before any introduction: complete the missing data and recalibrate the funding request |

The four pillars, and the reasoning behind each weighting:

| Pillar | Weight | The question it answers |
|---|---|---|
| Revenue & Growth | 30 | Is this a genuine business, and is it growing? |
| Jobs & Scale | 25 | Is the business trading, and will funding create employment? |
| Funding-Ask Viability | 25 | Is the requested amount proportionate to revenue, and is the application complete enough to be assessed? |
| Transformation (B-BBEE) | 20 | What is the B-BBEE level, and what is the extent of Black, women, youth and disability ownership? |

One principle governs all four pillars: **a missing response is scored as neutral, never as zero.** No business is penalised for a question the application form failed to capture properly.

### Portfolio outcome

| Tier | Businesses | Share | Avg. score | Avg. employees | Total requested |
|---|---|---|---|---|---|
| High | 273 | 24.5% | 70.5 | 23.5 | R4.89bn |
| Mid | 568 | 50.9% | 54.4 | 5.1 | R1.68bn |
| Low | 275 | 24.6% | 40.3 | 3.4 | R1.16bn |

Three findings from this segmentation shaped the recommended strategy.

**The High tier is comparatively small but immediately bankable.** These businesses have substantive operations, meaningful employment figures, and funding requests proportionate to their revenue. They represent an investor-ready pipeline that requires no further preparatory work, and they account for the majority of the total capital sought.

**The Mid tier is the centre of gravity of the portfolio.** It comprises approximately half of all applicants and represents the largest source of untapped pipeline value. These are genuine trading businesses whose shortcomings are specific and addressable. The appropriate response is structured development rather than rejection, and converting even a portion of this tier materially increases the number of businesses reaching investors.

**The Low tier presents two problems, both of which are remediable.** These businesses are characterised by funding requests disproportionate to their revenue and by incomplete application data. Both can be resolved through direct engagement with the applicant, which makes this tier a defined remediation workload rather than a write-off.

Transformation credentials are strong across the applicant pool as a whole. This is commercially significant, as B-BBEE standing and inclusive ownership are precisely the criteria many investors are mandated to apply. The scoring model surfaces these businesses rather than allowing them to remain undifferentiated within a pool of a thousand applications.

### Value delivered

The workflow converts an unranked applicant pool into a prioritised, decision-ready portfolio. Specifically, it:

- establishes a single documented definition of funding readiness that the team can apply consistently and explain to investors and applicants alike;
- reduces turnaround time by removing manual assessment from the critical path, so that an investor-ready shortlist is available on demand rather than after weeks of review;
- improves conversion by ensuring the businesses presented to investors are those most likely to meet investor mandates;
- identifies, quantifies and prioritises the development work required to convert the Mid and Low tiers, turning the remainder of the pool from a backlog into a managed pipeline;
- produces an auditable and reproducible record of how every business was assessed and ranked.

## The process

The workflow runs end to end from a single raw file. I wrote both notebooks as instructional documents: each code cell is preceded by a plain-language explanation, so that a non-technical reader can follow what is being done and why.

```
Datasets/capital_matching_applications.csv      raw form export — 1,186 rows × 27 columns
        │
        ├─ Python_Notebooks/Data_Cleaning.ipynb
        │     Rename 27 survey-question headers to tidy names (retained as a data dictionary).
        │     Strip underscore padding and parse monetary values into clean Rand figures.
        │     Cap implausible outliers (any value above R2bn is treated as a capture error).
        │     Convert revenue text-bands to an ordered 0–6 scale with Rand midpoints.
        │     Convert ownership percentage bands to numeric values; extract B-BBEE level.
        │     Standardise province spellings (KZN → KwaZulu-Natal); tidy cities and emails.
        │     De-duplicate: 68 exact copies and 2 re-submissions removed.
        ▼
Datasets/Capital_Matching_Cleaned_Data.xlsx     clean, analysis-ready — 1,116 × 41
        │
        ├─ Python_Notebooks/Funding_Readiness_Segmentation.ipynb
        │     Score the four pillars (0–1 each), apply weightings, and sum to a 0–100 readiness score.
        │     Assign a tier (High / Mid / Low) and rank every business, where 1 = most ready.
        ▼
Datasets/Funding_Readiness_Segmentation.xlsx    scored, tiered and ranked — 1,116 × 24
        │
        └─ SME_Capital_Matching_Dashboard.pbix   Power BI: tier, industry and province views
```

**Tools:** Python (pandas, NumPy, openpyxl) in Jupyter for cleaning and modelling; Excel as the hand-off format, as this is what the business already works in; Power BI for the reporting layer.

Both notebooks locate the `Datasets` folder automatically and execute from top to bottom, starting from the raw CSV. Re-running the pipeline on the same input reproduces the same output exactly, which means the segmentation can be refreshed as new applications arrive without any loss of consistency.

## About the data

**The dataset in this repository is synthetic.** The original applications contain real businesses, named individuals, email addresses, telephone numbers and company registration numbers, and therefore cannot be published. Every identifying value has been replaced with a fictitious equivalent.

The following characteristics of the original data have been deliberately preserved, as they are precisely what the workflow exists to address:

- the original 27 column headers, exactly as the form exported them;
- the same row count and duplicate structure (1,186 rows resolving to 1,116 unique businesses);
- the same categories of data quality problem, including underscore-padded amounts, revenue text-bands, inconsistent province names, malformed registration numbers, free text entered into numeric fields, and missing responses;
- realistic distributions, so that the model produces a portfolio shape comparable to the original.

The pipeline, the cleaning rules, the scoring model and the dashboard are the originals and run unchanged against this data. The two PDF documents were produced during the original engagement, and the figures they quote therefore differ slightly from the tier counts above, which are reproduced from the synthetic dataset.

[**Back to the Top**](#summary)
