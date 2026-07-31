# Stage 1 sources (documents / DPOC)

Methodology lives here rather than repeating on every CSV row. `source_key` in
`stage1_documents.csv` points to an entry below.

## 1. Three publications, one survey

Every national DPOC number we have traces to a **single fielding**. Citing Brennan and CDCE is
not two studies agreeing, it is one study cited twice.

| `source_key` | Publication | What it is |
|---|---|---|
| `cdce_voterid_2024` | [*Who Lacks ID in America Today?*](https://cdce.umd.edu/sites/cdce.umd.edu/files/pubs/Voter%20ID%20survey%20Key%20Results%20June%202024.pdf) (Rothschild, Novey, Hanmer, June 2024) | **Primary source.** Full DPOC section with race and party breakdowns |
| `cdce_2025` | [*Which U.S. citizens lack easy access to DPOC?*](https://cdce.substack.com/p/which-us-citizens-lack-easy-access) (April 2025) | Re-analysis of the same data, adds gender and age |
| `brennan_2025` | [Brennan Center analysis post](https://www.brennancenter.org/our-work/analysis-opinion/213-million-american-citizens-voting-age-dont-have-ready-access) | Summary of the above. Brennan co-funded the survey |

**Access note:** `cdce.umd.edu` serves an incomplete TLS certificate chain and fails in `curl`
and automated fetchers, though a browser gets through. The `cdce_2025` link above is the
Substack mirror, which is the version actually read. A newer PDF of the primary source exists as
`Voter ID 2023 survey Key Results June 2024 updatev2.pdf`; record whichever version you opened.

## 2. The data is from 2023

Fielded **September 12 to October 4, 2023**, which is roughly **2.8 years old** as of July 2026.
There is **no newer national DPOC survey** in this series. The 2024 fieldings are Georgia
(n=1,258) and Texas (n=1,210), both state-level, neither producing national figures.

Do **not** describe this as "current data." Say "national survey data, fielded fall 2023." It is
the best national DPOC estimate that exists, and stating the vintage is stronger than having a
judge find it.

---

## The national survey

| Field | Value |
|---|---|
| Administrator | SSRS |
| Sample | 2,386 adult U.S. citizens |
| Frame | SSRS probability panel plus random pre-paid cellular numbers |
| Field dates | September 12 to October 4, 2023 |
| Weighting | all results weighted; population counts from Census P20-586 (Nov 2022) |
| Oversamples | 18-24 year olds, Black respondents, Hispanic respondents, Black and Hispanic 18-24s, income under $30,000 |
| Funders | VoteRiders, Public Wise, CDCE (University of Maryland), Brennan Center for Justice |

### The measured definition, verbatim

DPOC means **U.S. birth certificate, U.S. passport or passport card, U.S. naturalization
certificate, or U.S. certificate of citizenship**. "Cannot readily access" covers both not having
the document at all and not being able to access it easily if needed.

Our `has_documents` variable measures **retrievability**, not ownership. That is the right
construct for the SAVE Act (which requires producing the document at registration), but the two
are not the same thing and the notebook should say so.

### Two known gaps

**Name change is not measured for DPOC.** It *is* measured for driver's licenses (1.5% have their
current address but not their current name). The DPOC questions never ask. Since the SAVE Act
re-triggers after a legal name change, the mechanism expected to affect the most women is absent
from our only Stage 1 source. The authors flag the female DPOC figure as a lower bound for this
reason. Part 4 discussion-only.

**Income is not broken out for DPOC.** Confirmed absent from the primary source. The report has
income cuts (`under $30k` / `$30-50k` / `$50-100k` / `over $100k`) but only for driver's licenses
and voter ID. **Do not substitute those for DPOC figures.** Different documents, different question.

### Denominator warning

The primary source reports DPOC on two different bases. Never mix them:

| Basis | Figures |
|---|---|
| All voting-age citizens | 9.1% / 21.3M cannot readily access; 1.6% / 3.8M lack any |
| Registered voters only | 6% / 10M have but cannot easily access; 1% / 2.1M no access |

The model starts from all eligible citizens, so the **all voting-age citizens** basis is correct
for the `has_documents` prior. Every row in the CSV carries a `denominator` column.

---

## For Srikar (Stage 2, physical access)

`cdce_voterid_2024` contains substantial physical-access data that is his stage, not Stage 1:

- 39% of people earning under $30,000 lack a license with current name/address; 23% have no license at all
- 20% of people self-identifying as having a disability have no license, vs 6% of non-disabled people
- 41% of 18-24 year olds lack a license with current name/address
- 21% of adults in strict-photo-ID states lack a current-name/address license

## Still needed

| For | Owner | Where |
|---|---|---|
| Stage 2 physical access | Srikar | CPS Voting and Registration Supplement, reasons for not voting |
| Baseline turnout and registration | Srikar | Census press release (73.6% registered, 65.3% turnout) |
| CPT conditionals, documents gate | Ty | Kansas *Fish v. Kobach* suspense list, 2013 to 2016 |
| CPT conditionals, access gate | Ty | CPS reasons for not voting, transportation and illness rates |

## Optional extension (not for today)

Georgia (n=1,258) and Texas (n=1,210), both fielded July 18 to August 11, 2024, could support a
state case analysis. Hook: nationally more Democrats than Republicans lack easy access, but in
Texas that reverses.
