# SME Capital Funding Optimization

**Sorting 1,116 funding applications into a ranked shortlist of the businesses an investor can fund today.**

The Johannesburg Stock Exchange runs a campaign called the SME Capital Matching Initiative, which introduces small and medium-sized businesses to investors who can provide them with capital to grow. In 2025 the campaign received more than 1,100 applications, which is far more than the team behind it can read one by one, so applications from businesses that were ready for funding went unseen.

This project corrects the errors in the submitted information and scores every application out of 100, using only what the applicant disclosed on the form. That number is the Funding Readiness Score. That score places each business into one of three groups, called tiers. **High** means the business can be taken to an investor exactly as it stands. **Mid** means it is a real trading business with specific gaps that can be closed. **Low** means the application needs work before anyone should see it. The team therefore works from a ranked list instead of an unsorted set of applications, and each early match strengthens the case for bringing further investors into the campaign.

Scoring the full pool placed **245 businesses in the High tier, 595 in the Mid tier and 276 in the Low tier**. The 245 in the High tier are, as a group, asking for less capital than they already earn, which is the clearest evidence available that a request is realistic.

### The Outcome

|            1,116            |                245                |        R10.30bn        |              26,506              |
| :--------------------------: | :--------------------------------: | :---------------------: | :------------------------------: |
| Businesses scored and ranked | In the High tier, ready to present | Total capital requested | New jobs the applicants forecast |

---

## Artifacts

The documents below contain the full argument and the full working.

| File                                                                                                                             | What it is                                                                                                  |
| -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| [**Insights &amp; Actionable Decisions (PDF)**](Documents/Reports/Insights_&_Actionable_Decisions.pdf)                      | What the scoring found, what to do about each tier, and the case for capital partners. Opens in the browser |
| [SME_Capital_Matching_Dashboard.pbix](SME_Capital_Matching_Dashboard.pbix)                                                        | The interactive Power BI report built on the scored data                                                    |
| [Business Case (PDF)](Documents/Reports/Business_Case.pdf)                                                                        | Why the campaign needed a readiness assessment, what I built, and what it produced                          |
| [Data Cleaning Walkthrough (PDF)](Documents/Reports/Data_Cleaning_Walkthrough.pdf)                                                | How the faulty data was diagnosed and repaired, step by step                                                |
| [Funding Readiness Scoring Methodology (PDF)](Documents/Reports/Funding_Readiness_Scoring_Methodology.pdf)                        | How the score is built, pillar by pillar, with two applications scored from beginning to end                |
| [Python_Notebooks/Data_Cleaning.ipynb](Python_Notebooks/Data_Cleaning.ipynb)                                                      | The cleaning code, with a plain-language write-up before every step                                         |
| [Python_Notebooks/Funding_Readiness_Segmentation.ipynb](Python_Notebooks/Funding_Readiness_Segmentation.ipynb)                    | The scoring code, with the full formula and a worked example                                                |
| [Datasets/capital_matching_applications.csv](Datasets/capital_matching_applications.csv)                                          | The raw, faulty application data as the form exported it                                                    |
| [Datasets/Capital_Matching_Cleaned_Data.xlsx](Datasets/Capital_Matching_Cleaned_Data.xlsx)                                        | The same data after cleaning                                                                                |
| [Datasets/Funding_Readiness_Segmentation.xlsx](Datasets/Funding_Readiness_Segmentation.xlsx)                                      | Every business scored, tiered and ranked                                                                    |
| [Download the entire project (.zip)](https://github.com/MainDevWork/SME_Capital_Funding_Optimization/archive/refs/heads/main.zip) | Every file in this repository, bundled as a single download                                                 |

## What the Dashboards Show

The scored list is the data source for a three-page report built in Power BI, which is Microsoft's tool for building reports that a reader can click through. Each tier keeps the same colour on every page, and the tier filter at the top of a page redraws that entire page for one tier at a time.

![Funding Readiness, Executive Summary](Documents/Images/executive_summary.png)

**Page 1, Funding Readiness.** A summary of all 1,116 businesses. The figures across the top give the number of businesses (1,116), the average readiness score (54.8 out of 100), the total amount of capital requested (R10.30bn) and the size of the High tier (245 businesses, or 22.0% of the pool). Below them are the split between the three tiers, the number of applicants in each province, and how many points each of the four parts of the score contributed on average.

![Scoring Dashboard, investor profiles](Documents/Images/funding_readiness_tiers.png)

**Page 2, Scoring Dashboard.** The comparison between the three tiers on the measures an investor asks about: revenue, amount requested, staff numbers, forecast new jobs and transformation credentials. It also contains a searchable list of every business, where clicking one row redraws the page for that single business.

![Contextual Profile](Documents/Images/contextual_profile.png)

**Page 3, Contextual Profile.** Where the applicants are based and what kind of businesses they are, covering applications over time, the number of businesses in each 2024 revenue band, and the breakdown by industry and by province within each tier. This is the page used to decide which industries and which provinces a targeted development effort should cover.

**Note.** The dataset published here is a stand-in, built to protect the confidentiality of the real applicants. The [About the data](#about-the-data) section explains exactly what was changed and what was preserved.

## Where the Data Comes From

Every figure in this project comes from a single file, which is the export of applications submitted through the JSE's [SME Capital Matching](https://www.jse.co.za/services/sme-rise/sme-capital-matching-initiative) online form. That export is the only input, and the two pieces of code and the dashboard are all built from it.

**What one row represents.** Each row is one funding application, filled in by the business itself through a public web form. No information was taken from credit bureaus or company registries, so the analysis uses only what the applicant chose to disclose, which is precisely the position the team is in when it decides who to put forward.

**Period and volume.** The applications run from **29 May 2025 to 26 June 2026**. The export holds **1,186 rows**, which resolve to **1,116 separate businesses** once duplicate and repeated submissions are removed.

**What the form collected.** The form captured **27 fields** per application. The ones that drive the analysis fall into five groups.

| Group                 | Fields used                                                         |
| --------------------- | ------------------------------------------------------------------- |
| Identity and location | Company name, registration number, province, city or town, industry |
| The funding request   | Amount requested, type of funding, purpose of the funding           |
| Financial history     | Annual revenue bands for 2023 and 2024, actual 2025 revenue         |
| Size and impact       | Current employees, forecast new jobs, number of shareholders        |
| Transformation        | B-BBEE level, and Black, women, youth and disability ownership      |

B-BBEE stands for Broad-Based Black Economic Empowerment, which is the South African framework that measures how far a business has moved towards Black ownership and inclusion. A business is rated from level 1, the strongest, downwards.

**The condition of the exported data.** The form produced tidy column headings but unusable values underneath them. Funding amounts came padded with trailing underscores, as in `250000_________`. Revenue was recorded as a text band rather than a number. One province appeared under two different names, `KZN` alongside `KwaZulu-Natal`. Registration numbers were malformed, free text was entered in fields meant to hold numbers, submissions were duplicated, and fields that investors insist on were left blank. Roughly one field in every ten was unusable as captured. Repairing that data, without discarding businesses for gaps the form itself created, is the first half of the work, and scoring what remains is the second.

**On reproducing these figures.** The numbers quoted on this page come from the live application data, which is confidential and is not published here. The code is committed alongside a stand-in dataset that has the same columns, the same number of rows and the same kinds of error, so the code runs from beginning to end for anyone who copies the repository. The tier counts that stand-in produces, **273, 568 and 275**, will not match the figures quoted here, which are **245, 595 and 276**. The scoring formula, the weightings and the tier boundaries are identical in both cases.

## How It Works

The workflow runs from the raw file to the dashboard in two documented notebooks, which are files that hold the code and the explanation of the code side by side. The first notebook writes an Excel file and the second one reads it. Every block of code is preceded by a plain-language explanation of what it does and why, so a reader who does not write code can still follow the reasoning, and running them again on the same input reproduces the same output exactly.

```
Datasets/capital_matching_applications.csv      raw form export, 1,186 rows by 27 columns
        │
        ├─ Python_Notebooks/Data_Cleaning.ipynb
        │     Rename the 27 survey headings to short names, kept as a dictionary of fields.
        │     Strip the underscore padding and turn the money text into clean Rand figures.
        │     Cap impossible outliers, treating any value above R2bn as a capture error.
        │     Turn revenue text bands into an ordered 0 to 6 scale with a Rand value for each.
        │     Turn ownership bands into numbers, and pull out the B-BBEE level.
        │     Standardise province spellings, so that KZN becomes KwaZulu-Natal.
        │     Remove duplicates: 68 exact copies and 2 repeat submissions.
        ▼
Datasets/Capital_Matching_Cleaned_Data.xlsx     clean and ready to analyse, 1,116 by 41
        │
        ├─ Python_Notebooks/Funding_Readiness_Segmentation.ipynb
        │     Score four pillars, weight them, and add them into a score out of 100.
        │     Assign a tier and a rank to every business, where rank 1 is the most ready.
        ▼
Datasets/Funding_Readiness_Segmentation.xlsx    scored, tiered and ranked, 1,116 by 24
        │
        └─ SME_Capital_Matching_Dashboard.pbix   the Power BI report
```

### Step 1: Cleaning the Data

The steps listed above turn the faulty export into a table the scoring can trust, and the recovery is the clearest measure of what they achieve. Usable funding amounts rise from **10 to 1,109**, and usable 2025 revenue figures from **none at all to 1,112**, without a single business being discarded for a gap the form itself created. Each decision, including why an amount above R2 billion is treated as a typing error rather than a real request, is explained in the [Data Cleaning Walkthrough](Documents/Reports/Data_Cleaning_Walkthrough.pdf).

### Step 2: The Scoring Model

Each business receives a **Funding Readiness Score from 0 to 100**. The score is built from four pillars, where a pillar is one of the four subjects the score measures, and all four use only fields the form already collects. Each pillar produces a value between 0 and 1, that value is multiplied by the weight given to the pillar, and the four results are added together.

| Pillar                  |    Weight    | The question it answers                        | How it is scored                                                                                                                                                               |
| ----------------------- | :----------: | ---------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Revenue and growth      | **30** | Is this a real business, and is it growing?    | 60% from the size of the latest revenue band, and 40% from whether revenue moved up a band between 2023 and 2024                                                               |
| Jobs and scale          | **25** | Is it trading, and will funding create jobs?   | 55% from current staff and 45% from forecast new jobs, counted on a scale that increases quickly at low numbers and awards full marks from 50 upwards                          |
| Is the request viable   | **25** | Is the amount sensible, and the form complete? | 60% from how the amount requested compares with revenue, treating half of revenue up to five times revenue as sensible, and 40% from how many of ten key fields were filled in |
| Transformation (B-BBEE) | **20** | What are the transformation credentials?       | 50% from the B-BBEE level, and 50% from the average of the four ownership percentages                                                                                          |

**The weightings follow what funders act on.** Revenue and growth carries the most weight because it is the clearest evidence that a genuine business exists. Jobs and the viability of the request follow, because a proportionate request from a business that already employs people is what a funder can actually structure a deal around. Transformation carries the least weight, not because it matters least, but because almost every applicant scores well on it, with Black ownership averaging 91.6% across the pool, so it separates one applicant from another far less than the other three pillars do. It is kept in the score because many funders are required by their own mandates to apply it.

**One fairness rule applies to every pillar: an unanswered question is scored as neutral, at 0.5, and never as zero.** No business receives a lower score because of a question the form failed to capture. There are two deliberate exceptions, both cases where a blank genuinely means none, and those are a reported revenue of nothing, which is treated as a business that has not yet started earning, and a blank staff or jobs figure, which is treated as no employment recorded.

### Step 3: Sorting Into Tiers

Fixed score boundaries turn the score into the three tiers, so that a tier has the same definition when a future round of applications is scored.

| Tier           | Score     | What it means                                                              | What to do about it                                                           |
| -------------- | --------- | -------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| **High** | 62 to 100 | Established, credible businesses asking for an appropriately sized amount  | Package and present to investors immediately                                  |
| **Mid**  | 48 to 61  | Genuine trading businesses with specific gaps that can be closed           | Develop: provide readiness support, right-size the request, and score again   |
| **Low**  | 0 to 47   | Early-stage businesses, or applications missing information investors need | Repair before any introduction: complete the data and recalculate the request |

Every business also receives a rank, where 1 is the most ready, and ties are settled alphabetically so that the same data always produces the same file. The full formula for each pillar, with one business traced through it from beginning to end, is documented block by block inside [Funding_Readiness_Segmentation.ipynb](Python_Notebooks/Funding_Readiness_Segmentation.ipynb).

## Tools Used

| Tool                             | What it does here                                   |
| -------------------------------- | --------------------------------------------------- |
| **Python** (pandas, NumPy) | Cleaning, transformation and the scoring model      |
| **openpyxl**               | Writing the Excel outputs, colour-coded by tier     |
| **Jupyter Notebook**       | The documented, repeatable workflow for both stages |
| **Microsoft Excel**        | The handover format the team already works in       |
| **Power BI**               | The three-page report built on the scored data      |
| **Git and GitHub**         | Version control and publication of the project      |

## What the Scoring Found

Scoring all 1,116 businesses divides them into three clearly different groups. The High tier is small but immediately fundable, the Mid tier contains most of the applicants, and the Low tier is a defined body of repair work rather than a set of rejections. Across the whole pool the businesses request **R10.30bn** of capital, average a readiness score of **54.8 out of 100**, and forecast **26,506** new jobs.

| Tier            | Businesses      | Share          | Total requested    | Total revenue     | Avg. staff | Avg. new jobs | Black ownership |
| --------------- | --------------- | -------------- | ------------------ | ----------------- | ---------- | ------------- | --------------- |
| High            | 245             | 22.0%          | R3.24bn            | R4.05bn           | 23.2       | 47.1          | 95.4%           |
| Mid             | 595             | 53.3%          | R2.66bn            | R1.77bn           | 4.6        | 20.1          | 92.3%           |
| Low             | 276             | 24.7%          | R4.40bn            | R1.80bn           | 2.9        | 14.0          | 86.8%           |
| **Total** | **1,116** | **100%** | **R10.30bn** | **R7.62bn** |            |               | 91.6%           |

**1. The High tier carries the lowest risk.** Only 22% of applicants reach it, yet it is the only tier whose businesses together ask for less than they earn, at **R3.24bn** requested against **R4.05bn** of revenue. These businesses employ roughly **5,690** people between them, forecast about **11,436** new jobs, and hold the strongest transformation credentials in the pool at **95.4%** average Black ownership. This is a shortlist that needs no further preparation before an investor sees it. It is also larger than the number of businesses the campaign matched with funders in previous rounds, which was 90 in the 2023 pilot and 190 in 2024, although a ready shortlist and a completed match are not the same measure.

**2. The businesses in the Low tier ask for the most and earn the least.** Against expectation, the least ready tier has the **largest total request of all, at R4.40bn**, against just **R1.80bn** of revenue, and it comes from the smallest businesses in the pool at an average of 2.9 staff. Its typical request of **R800,000** is more than double the Mid tier's **R323,500**, taking the middle request in each tier so that a few very large requests do not distort the comparison. These are requests out of all proportion to trading history, which no funder can support as written, so bringing the request down to a realistic size is the single most valuable intervention available for the lower tiers.

**3. Forecast job creation is substantial and concentrated.** The pool forecasts **26,506 new jobs** in total, and the High tier alone accounts for roughly **11,436** of them, at an average of **47 per business**. Because those businesses already employ people at scale, it is also the tier most credible to deliver on the forecast, which is what makes the employment and transformation case that development finance investors are mandated to fund.

**4. Transformation credentials are strong across the entire pool.** Average Black ownership is **91.6%**, and every tier scores well on the transformation pillar, at **15, 14 and 13 out of 20** for High, Mid and Low. This matters commercially, because B-BBEE standing and inclusive ownership are exactly the criteria many investors are required to apply, and the scoring identifies these businesses individually instead of leaving them undifferentiated among more than a thousand applicants.

**5. The share of ready businesses stayed steady as the number of applications rose and fell.** The High tier held a steady **17% to 28%** share of every month's applications. Investor-ready businesses arrived in all fourteen months, and two thirds of them, **161 of the 245**, arrived from September 2025 onwards. Matching should therefore run continuously, rather than waiting for the campaign to close.

**6. Applications concentrate in three provinces.** **Gauteng, the Western Cape and Limpopo** account for the large majority of the pool, and the same three dominate the High tier. This tells the team where an in-person or targeted development effort would reach the most businesses.

## What to Do With Each Tier

The tiers are not a queue to be worked from the top down. Each one calls for a different action, and running all three at the same time is what produces the most funded deals.

**Take the High tier to investors now.** These businesses are ready as they stand, so the work is packaging them and matching them to investors by industry, province and transformation profile. This is where the fastest deals will come from, and each one is an example the team can show to prospective investors.

**Develop the Mid tier.** At more than half the pool, this is the largest group of businesses that could be made ready with support, and their shortcomings are specific and correctable rather than fundamental. The right response is structured support followed by a fresh score, not rejection, and converting even a portion of this tier materially increases the number of businesses reaching investors. It therefore warrants the bulk of the team's development effort.

**Repair the Low tier over time.** Both of this tier's problems, the oversized request and the incomplete application, are resolved through direct contact with the business, after which it is scored again and can move up. That makes it a defined and prioritised body of work for later rounds rather than a write-off.

**Use the scored list to attract investors.** Beyond its use as an internal priority list, the scored pool is evidence in its own right, being a quantified list of transformation-strong businesses that the team can put in front of prospective funders to draw new capital into the campaign. The [Insights &amp; Actionable Decisions](Documents/Reports/Insights_&_Actionable_Decisions.pdf) deck is that argument, built to be presented as it is.

## Limitations and Assumptions

The model is deliberately transparent, which means its assumptions should be read alongside its results.

- **The data is self-reported and unverified.** Every figure is what the applicant typed in, and nothing is confirmed against registries, financial statements or credit records. The score measures readiness to be presented, not creditworthiness, and a funder's own checks still follow.
- **Revenue arrives in bands.** The 2023 and 2024 revenue figures are ranges rather than exact amounts, so the model uses a representative value from the middle of each range. That is precise enough to rank and to sort into tiers, but any individual comparison of request against revenue is approximate.
- **Unanswered questions are neutralised rather than penalised.** Blanks score 0.5 so that no business receives a lower score because of the form's own gaps. The trade-off is that a genuinely incomplete application can score much like a modest but complete one, which is what the completeness component of the score and the Low tier repair step exist to limit.
- **The weights and boundaries are fixed choices.** The 30, 25, 25 and 20 pillar weights and the 62 and 48 tier boundaries encode a defensible view of what funding readiness means, but they remain choices. They are in one place in the code and can be changed as the campaign learns what actually converts.
- **The model caps extremes on purpose.** Amounts above R2bn are treated as capture errors, the comparison of request against revenue stops increasing beyond the sensible range, and staff and job counts award full marks from 50 upwards. These caps prevent a small number of extreme entries from changing the order of the whole list.

## Next Steps

- **Run each new round through the same workflow.** Because it is fully repeatable, every future intake can be cleaned, scored and sorted on demand, which keeps a ready shortlist available at all times instead of after weeks of manual reading.
- **Record which tiers actually convert.** As deals close, record which tier each funded business came from, and adjust the weights and boundaries against that record.
- **Track businesses across rounds.** Scoring again shows which Mid and Low tier businesses have moved up after development support, so the model also records progress over time.
- **Add more information to the model.** Where consent and access allow, verified financial statements or registry checks could be added to the self-reported fields to make the upper tiers more accurate.

### About the Data

**The dataset published in this repository is a stand-in, not the real applications,** although the workflow applied to it is the one that ran on the original. The original applications contained real business names, named individuals, email addresses, telephone numbers and company registration numbers, and therefore cannot be published. Every identifying value has been replaced with a fictitious equivalent.

The following characteristics of the original were preserved on purpose, because they are precisely what the workflow exists to deal with:

- The original 27 column headings
- The same row count and duplicate structure, at 1,186 rows resolving to 1,116 separate businesses
- The same categories of data-quality problem
- A similar split across the three tiers, so that the number of businesses in each tier is comparable

The cleaning, the scoring model and the dashboard all run unchanged against this stand-in. **The dashboard screenshots and the headline figures on this page come from the original data,** and were checked to ensure that no confidential detail is visible in them.

## Author and Contact

**Ndivhuwo**, data analysis, modelling and dashboard.

- Email: [ndivhuwojse@gmail.com](mailto:ndivhuwojse@gmail.com)
- LinkedIn: [www.linkedin.com/in/ndivhuwo-makhavhu](https://www.linkedin.com/in/ndivhuwo-makhavhu)
- GitHub: [github.com/MainDevWork](https://github.com/MainDevWork)
- Project: [SME_Capital_Funding_Optimization](https://github.com/MainDevWork/SME_Capital_Funding_Optimization)

[**Back to the top**](#sme-capital-funding-optimization)
