# jstor-dda-hlm

Build the weekly EBSCO Holdings Management (HLM) upload file for the JSTOR
Demand-Driven Acquisition (DDA) program from the raw GOBI reports.

This repository holds scripts and documentation only. No patron data, holdings
exports, or upload files are committed. Input and output folders live in
SharePoint, not in Git.

## What it does

GOBI sends weekly JSTOR DDA reports (additions, deletions, transactions) to
`libacq@okstate.edu`. This script reads those raw `.xlsx` files and writes one
CSV formatted for HLM, applying the package routing rules from the DRDS
procedure. A student worker can run it in a few minutes and repeat it each week.

## Repository layout

```
jstor-dda-hlm/
  README.md                         Overview and quick start (this file)
  R/
    build_jstor_dda_hlm_upload.R    Build step: raw GOBI files -> upload CSV
    check_upload_against_hlm.R      Review step: flag likely HLM match problems
  docs/
    PROCEDURE.md                    Full DDA upload procedure (source of truth)
    RUNBOOK.md                      Step-by-step run and troubleshooting guide
    PREFLIGHT_CHECK.md              The optional pre-upload check explained
    DATA_DICTIONARY.md              Source columns in, five columns out
    CHANGELOG.md                    Dated record of changes to this workflow
```

## Quick start

1. Install the packages once:
   ```r
   install.packages(c("readxl", "readr", "dplyr", "stringr"))
   ```
2. Save this week's raw GOBI `.xlsx` files in the SharePoint `INPUT_FOLDER`.
   File names must contain `ADDITION`, `DELETE`, or `TRANSACTION`.
3. Open `R/build_jstor_dda_hlm_upload.R` in RStudio and click Source.
4. Read the printed summary. The output file
   `JSTOR_DDA_HLM_UPLOAD_YYYYMMDD.csv` lands in `OUTPUT_FOLDER`.
5. Upload the CSV in HLM (Format: Full Text Finder, one email address), then
   update the JSTOR DDA Upload Log list row.

See `docs/RUNBOOK.md` for the full run, and `docs/PROCEDURE.md` for the routing
rules the script follows.

## Scope

The script formats the file. It does not decide which titles are DDA-eligible.
Confirm each title in the correct JSTOR package in HLM before uploading, and wait
one full week before uploading additions. See Pre-Upload Validation in the
procedure.

## Contacts

- Workflow owner: Rebekah Silverstein, rebekah.silverstein@okstate.edu
- Acquisitions: Gala Lackey, gala.lackey@okstate.edu
- Reports inbox: libacq@okstate.edu
- HLM errors: open a case via the EBSCO Admin portal
# jstor-dda-hlm
Build the weekly EBSCO HLM upload file for JSTOR DDA
