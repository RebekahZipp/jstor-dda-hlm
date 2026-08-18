# jstor-dda-hlm

Build the monthly EBSCO Holdings Management (HLM) upload file for the JSTOR
Demand-Driven Acquisition (DDA) program from the raw GOBI reports.

GOBI emails weekly DDA reports (additions, deletions, transactions) to
`libacq@okstate.edu`. These scripts turn those raw `.xlsx` files into one CSV
formatted for HLM, and check the results. A student worker can run the whole
cycle in a few minutes and repeat it each month.

**Scripts and documentation only. No patron data, vendor reports, upload files,
or HLM exports are committed.** Data lives in SharePoint; Git holds the logic.

## Scripts

| Script | When | Does |
|--------|------|------|
| `R/build_jstor_dda_hlm_upload.R` | Build | Raw GOBI files to one upload CSV |
| `R/check_upload_against_hlm.R` | Before upload | Flags rows likely to fail, from a current HLM export |
| `R/read_hlm_results.R` | After upload | Turns HLM's results export into a manual worklist |

The results export exists only after an upload finishes, so `read_hlm_results.R`
is always the last step. The pre-flight check reads a *holdings* export before
upload; the results reader reads the *Matching Status* export after. Two
different HLM downloads.

## Docs

| Doc | Read it for |
|-----|-------------|
| `docs/PROCEDURE.md` | The routing rules, source of truth |
| `docs/RUNBOOK.md` | Running the monthly cycle, step by step |
| `docs/PREFLIGHT_CHECK.md` | Why uploads bounce and how the checks catch it |
| `docs/DATA_DICTIONARY.md` | Every column, in and out |
| `docs/SETUP_GITHUB.md` | Building and maintaining this repo |
| `docs/CHANGELOG.md` | What changed and why |

## Quick start

```r
install.packages(c("readxl", "readr", "dplyr", "stringr"))
```

1. Put this cycle's raw GOBI `.xlsx` files in the SharePoint `INPUT_FOLDER`.
   File names must contain `ADDITION`, `DELETE`, or `TRANSACTION`.
2. Source `R/build_jstor_dda_hlm_upload.R`. Output lands in `OUTPUT_FOLDER` as
   `JSTOR_DDA_HLM_UPLOAD_YYYYMMDD.csv`.
3. Optional: download a current HLM export and source
   `R/check_upload_against_hlm.R` to catch problems before upload.
4. Upload the CSV in HLM (Format: Full Text Finder, one email).
5. Download HLM's results export and source `R/read_hlm_results.R` to get the
   worklist of titles to finish by hand. Update the JSTOR DDA Upload Log row.

## Routing, in one line each

- Addition: `Books at JSTOR: Demand-Driven Acquisition`, DELETE blank.
- Deletion: `Books at JSTOR: Demand-Driven Acquisition`, DELETE Y.
- Transaction: two rows, `Books at JSTOR` blank **and** the DDA package DELETE Y.

The script does not decide eligibility. Confirm titles in HLM first, and wait a
week before uploading additions. See `docs/PROCEDURE.md`.

## Contacts

- Workflow owner: Rebekah Silverstein, rebekah.silverstein@okstate.edu
- Acquisitions: Gala Lackey, gala.lackey@okstate.edu
- Reports inbox: libacq@okstate.edu
- HLM errors: EBSCO Admin portal
# jstor-dda-hlm
Build the weekly EBSCO HLM upload file for JSTOR DDA
