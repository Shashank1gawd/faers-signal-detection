# FAERS Signal Detection Dashboard

A single-file, browser-based pharmacovigilance (PV) tool that screens **FDA FAERS** adverse-event reports for possible drug–event safety signals using standard disproportionality statistics (PRR, ROR, IC, EBGM approximations), then lets you drill into the case-level reports behind a signal.

> **Educational prototype. Not for clinical, regulatory, or patient-care decisions.** Every result needs review by a qualified PV/clinical professional.

**This is Project 1 of a 2-project PV portfolio.** Its companion is [icsr-case-workbench](../icsr-case-workbench) (Project 2), where the supporting cases from a signal are processed one by one.

---

## Live demo

After you enable GitHub Pages (see [Deploy](#deploy-on-github-pages)):
`https://YOUR-USERNAME.github.io/faers-signal-detection/`

## Use case

A safety scientist or PV trainee wants a quick first look at a question such as *"Is angioedema reported disproportionately often with drug X compared with other drugs in FAERS?"*

Typical workflow:
1. Enter a drug (and optionally an event) and run the analysis on live FDA data.
2. Review which drug–event pairs cross the signal rule and how many of the four methods agree.
3. Open **Investigate** to read the case-level picture (seriousness, outcome, dechallenge/rechallenge, fatal cases, etc.).
4. Send the case set to Project 2 for case-by-case processing, then import the feedback back here.

## Features

The app has four tabs:

| Tab | What it does |
|---|---|
| **Live data** | Queries the public openFDA drug-event API, ranks drug–event pairs, applies signal criteria, shows method agreement, and offers *Copy as CSV* and *Export report*. Recently searched drugs are kept as quick chips. Each result row can be marked *Not reviewed / Confirmed / Dismissed* with a note. |
| **Investigate** | Pulls a sample (up to 100 most recent reports) for one drug–event pair and breaks it down: seriousness, outcome, sex, age group, reporting-year trend (sample only), indication, concomitant drugs, dechallenge, rechallenge, fatal-case review, and a case-level table. Includes the hand-off to Project 2 and the feedback import. |
| **Calculator** | Manual 2×2 table calculator: enter a, b, c, d for one or more drugs and get the same statistics. Works offline, no API. |
| **Learn** | Definitions, the 2×2 table, all formulas, the signal rule, a worked example, limitations, what was left out on purpose, and a glossary. |

### Statistics and signal rule

Built from the 2×2 table (a = reports with drug and event, b = drug without event, c = event with other drugs, d = other drugs without event):

- **PRR**, **ROR** (with confidence intervals), **χ²**
- **IC~** and **EBGM~**: simple approximations, marked with `~`, not the full Bayesian models used in production systems
- **Signal rule (Evans criteria):** `a ≥ 3` and `PRR ≥ 2` and `χ² ≥ 4`
- **Method agreement:** how many of the four methods independently exceed their own rule-of-thumb threshold (e.g. "3/4 agree")

Worked example in the app: a=20, b=80, c=30, d=370 gives PRR 2.67, ROR 3.08, χ² 13.9, IC~ 0.97, EBGM~ 1.95, which clears the Evans rule.

## How to use

1. Open `index.html` in a modern browser (Chrome, Edge, Firefox, Safari), or use the Pages link above. An internet connection is needed for the **Live data** and **Investigate** tabs; the **Calculator** and **Learn** tabs work without it.
2. On **Live data**, type a drug name (try both brand and generic spellings) and press **Run analysis**. Use **Check this one** for a single drug–event pair.
3. Read the results table. Expand a row to add a review status and note.
4. Go to **Investigate** to see the case-level breakdown for a pair.
5. Use the **Calculator** tab if you already have your own 2×2 counts.

### Connecting to Project 2 (signal → case processing → feedback)

The two apps are separate static sites, so they exchange data by **copy and paste** (browser storage is not shared between sites):

1. In **Investigate**, use **Copy case set for Project 2**. This copies text starting with `PVCASES:`.
2. In Project 2's **Signal** tab, paste it and press **Load signal cases**.
3. Process the cases in Project 2, then use **Copy feedback for Project 1** there (text starting with `PVFEEDBACK:`).
4. Back here, paste it into the feedback box and press **Import feedback**. It is only accepted if the drug and event match the current signal.

## Data source

[openFDA drug adverse event API](https://open.fda.gov/apis/drug/event/), which exposes public FAERS data. No API key, login, or backend is used.

## Data provenance labels

Information carried into Project 2 is tagged as *Reported*, *Inferred*, *Auto-prefilled* or *Manual* there, so a reviewer can see what still needs verification.

## Limitations

- FAERS is a **spontaneous reporting** system: it cannot give true incidence rates, and a signal is a hypothesis, not proof of causality.
- One report can list several drugs or reactions, so counts overlap.
- The case sample is capped at **100 reports** (API limit for non-aggregate queries) and is the most recent, not random. Reporting-year trends refer to the sample only.
- Duplicates, media attention, stimulated reporting and confounding by indication can create false signals.
- Drug names vary; results depend on how the name was entered.
- Analysis is at **Preferred Term (PT)** level only. The full MedDRA SOC/HLGT/HLT hierarchy needs a licensed MedDRA dictionary that openFDA does not expose.
- IC~ and EBGM~ are simplified approximations.
- Reviewer notes and recent searches are stored in your browser's `localStorage` only: they are private to that browser, not shared, and are lost if you clear site data. This is a lightweight stand-in for a real reviewer workflow and audit trail.
- openFDA applies usage limits to unauthenticated requests; if requests start failing, wait a while and retry.
- Not a validated system. It has no regulatory compliance features (no audit-grade access control, no electronic signatures).

## Deliberately not included

- AI case summarisation (would need a live AI API call, which a static page cannot make safely)
- Full MedDRA hierarchy (licensed terminology)
- Literature integration and formal audit/compliance sign-off (need a real backend and access control)

## Deploy on GitHub Pages

See [docs/DEPLOY.md](docs/DEPLOY.md). Short version: push this repo, then **Settings → Pages → Deploy from a branch → `main` / root**.

## Repository layout

```
faers-signal-detection/
├── index.html        # the whole app (HTML + CSS + JS, no build step)
├── README.md
├── LICENSE
├── .gitignore
└── docs/
    ├── DEPLOY.md     # GitHub upload + Pages steps
    └── PV-CONCEPTS.md# short PV primer for reviewers/recruiters
```

## Tech

Plain HTML, CSS and vanilla JavaScript in one file. No dependencies, no build step, no server.

## Disclaimer

Educational prototype only. Not medical advice, not a validated pharmacovigilance system, and not suitable for regulatory submissions or patient-care decisions.

## License

MIT, see [LICENSE](LICENSE). Data from openFDA is subject to FDA's terms.

## Author

YOUR NAME · [LinkedIn](https://www.linkedin.com/in/YOUR-PROFILE) · [GitHub](https://github.com/YOUR-USERNAME)
