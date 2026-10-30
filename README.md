# 🍁 Maple Seed Funding Guide

> One place where Canadian small and medium-sized businesses can **find, understand and prepare for** the government and other support programs that fit them.

**Maple Seed Funding Guide** (in French: *Maple Seed – Guide de financement*) is an independent, non-commercial learning project in product design and object-oriented software engineering. It is bilingual (English for Canada, French for Québec) and is being built as a portfolio project.

> ⚠️ **Independent project.** Maple Seed Funding Guide is not a Government of Canada website and is not affiliated with, endorsed by or sponsored by any government, financial institution or educational institution. It does not give legal, financial or tax advice and never applies for funding on anyone's behalf. Always confirm criteria, amounts and deadlines on each program's official website.

---

## 🔍 The Problem

Small and medium-sized businesses are the backbone of Canada's economy, and support for them exists at scale. Yet few small businesses use government financing, and awareness of specific programs is low.

| Finding | Figure |
| --- | --- |
| Employer businesses in Canada (Dec. 2024) | 1.10 million, 98.2% of them small (1–99 employees) |
| Share of private-sector jobs held by SMEs (2024) | 63.6% (about 8 million people) |
| Small businesses that requested government financing in 2024 | 1% (against 24% for trade credit and 9% for debt) |
| Government financing requests that were approved | 79% |
| Non-users of the Canada Small Business Financing Program who knew it existed | 16% |

Support comes from many places: federal, provincial and territorial governments, municipalities, non-profits and the private sector. The information is public, but each source uses its own words and steps.

**Reading the data honestly:** 79% of small businesses said they did not need financing in 2024, and no national figure measures how hard programs are to find. This project targets owners who *do* need support and **assumes** that finding and understanding programs is hard for them. That assumption has not been measured yet.

*Sources: ISED, Key Small Business Statistics 2025; ISED, Credit Conditions Survey 2024; ISED, Borrower Awareness and Satisfaction Study 2023–2024. Figures read in October 2026.*

### Pain points to validate

1. "I do not know which programs exist for my business."
2. "I cannot tell whether I qualify."
3. "Every program site uses its own words and steps."
4. "I do not know what to prepare before applying."
5. "A wrong guess costs time I do not have."

---

## 💡 The Solution: Find, Understand, Prepare

| Step | Feature | What it does |
| --- | --- | --- |
| **Find** | Search by Filters | A program directory with filters for provider, type of support, goal, amount, sector, province, size, revenue, structure, owner profile and availability |
| **Understand** | Find My Support | Five short questions (about 2 minutes, no account needed). Programs are grouped into **Match**, **Maybe** and **Not a match**, with the reason for each criterion |
| **Prepare** | My list and PDF report | Save programs, tick off a checklist of what to prepare, and download a report to share with an accountant |

### Principles

- **Rules, not AI.** Matching follows each program's published criteria. No black box.
- **Always explained.** Every result says what fits, what to confirm and what does not.
- **Honest.** No promises, no advice; every program links to its official website.
- **Private by design.** Nothing is saved unless the user chooses. Designed with PIPEDA and Québec's Law 25 in mind.
- **Bilingual** from the first version.
- **Independent** and non-commercial.

---

## ⚙️ How the Matching Works

```text
Business profile ──► Program rules ──► Check each criterion ──► Result
(province, sector,   (published        (Fits / To confirm /      (Match / Maybe /
 size, goal,          criteria,         Does not fit)             Not a match,
 structure)           entered as data)                            with reasons)
```

- Any criterion that does not fit → **Not a match**.
- Only "to confirm" items → **Maybe**.
- Everything fits → **Match**.

Each criterion type (province, size, sector, structure, goal, revenue) is a subclass of an abstract `EligibilityCriterion` with its own `evaluate(profile)` method. Rules are data, so a curator can change them without changing code. Results are only as good as the rules, which is why the curator role exists.

---

## 👥 Users

| Actor | Role |
| --- | --- |
| **Visitor** | An SME owner browsing without an account |
| **Registered user** | A visitor with an account: saved list, business profile, PDF report, data export and deletion |
| **Content curator** | Imports program data and keeps eligibility rules current in an internal console. Curator accounts are invitation-only and require two-step verification |

Programs move through three statuses: **Needs review** (new or changed in an import) → **Active** (rules checked by the curator) → **Closed**. Only Active programs are shown to business owners.

---

## 🏛️ Architecture

A three-tier web application in the Model-View-Controller style, with five subsystems:

| Subsystem | Responsibility | Use cases |
| --- | --- | --- |
| **Catalogue** | Programs, providers, filters and program pages | Search, view program details |
| **Matching** | Rule engine: evaluate a profile and group the results | Find My Support, recommendations |
| **Reporting** | PDF report in English or French | Generate PDF report |
| **Accounts** | Sign-up, consent, login, roles and access control, data export and deletion | Account, profile, list, privacy, curator login |
| **Data import** | Pipe-and-filter pipeline: download → clean → normalise → validate → store | Import program data, maintain rules |

- **Repository style** for program data: Catalogue, Matching and Reporting read the same program store.
- **Personal data is kept in a separate store** from public program data, so deleting a user touches only that store.

The full analysis (use cases, UML class, sequence and state machine diagrams, and architecture decisions) is in the design document in `docs/`.

---

## 🕒 Current Stage

| Phase | Content | Status |
| --- | --- | --- |
| Product design | Clickable prototype in English and French, style guide, brand and logo | ✅ Done |
| Curator console | Program catalogue, import review, rule editor, two-step login | ✅ Designed |
| Software design | 13 use cases, UML analysis model, proposed architecture | 🟡 First draft |
| MVP | Data import, rules for a first set of programs, matching engine with unit tests, saved list and PDF report | ⏭️ Next |
| Polish and share | Accessibility checks (WCAG 2.1 AA), deployed demo, case study, informal feedback from business owners | Later |

The scope stays small on purpose: a working, well-documented MVP matters more than covering every program.

---

## 🛠️ Tech Stack (planned)

- **Frontend:** HTML, CSS, JavaScript / React
- **Backend:** Python or Java *(to be decided)*: API, rule engine, data import pipeline
- **Database:** PostgreSQL
- **Version control:** Git and GitHub

---

## 📂 Project Structure (planned)

```text
maple-seed-funding-guide/
│
├── docs/              # Design document, UML diagrams, research notes, brand record
├── frontend/          # Bilingual views (EN/FR) and the curator console
├── backend/
│   ├── catalogue/     # Programs, providers, search and filters
│   ├── matching/      # Rule engine: EligibilityCriterion subclasses, MatchResult
│   ├── reporting/     # PDF report generation
│   ├── accounts/      # Users, roles, consent, data export and deletion
│   └── data_import/   # Pipeline: download, clean, normalise, validate, store
├── database/          # PostgreSQL migrations and schemas
└── tests/             # Unit, integration and acceptance tests derived from the use cases
```

---

## 📊 Data Source and Licence

Program information is adapted from the **Business Benefits Finder** dataset published by Innovation, Science and Economic Development Canada (ISED). Contains information licensed under the [Open Government Licence – Canada](https://open.canada.ca/en/open-government-licence-canada).

The prototype uses example programs and simplified rules. Real criteria must be copied from each program's official website.

---

## 👩‍💻 Author

**Katherine Borges Sato**: independent, non-commercial learning project.
## 📂 Project Architecture

```text
can-business-support-app/
│
├── docs/            # Research notes, benchmark analysis, and user personas
├── scripts/         # Web scrapers and data normalization scripts for public/private sources
├── frontend/        # Tabular B2B dashboard and Corporate Triage UI
├── backend/         # Eligibility scoring engine, API routing, and deterministic logic
└── database/        # PostgreSQL migrations, table schemas, and indexed subsidy data
```
