
## Install

```bash
python -m venv .venv
source .venv/scripts/activate
pip install -r requirements.txt
python -m playwright install chromium
```

## Project Overview

This project collects public meeting minutes and California Form 700 financial disclosure filings for a selected jurisdiction, stores them in a local database, and converts them into structured data for analysis. It downloads minutes PDFs, extracts vote and meeting information, syncs Form 700 filings from the FPPC portal, and parses disclosed entities such as businesses, properties, lenders, and income sources.

The scraper also compares full minutes text against entities disclosed by politicians in the selected jurisdiction and exports the results as CSV and JSON files. A desktop interface is included for configuring searches, running live or one-time scrapes, and reviewing outputs, making the project a complete workflow for monitoring public records and identifying possible overlaps between government actions and reported financial interests.


## (How to Run)  
Run the PyQt5 desktop app (recommended):

```bash
python civic_vote_scraper_desktop_app_registry.py
```

One-time run:

```bash
python -m civic_vote_scraper.cli --url "https://sonoma-county.legistar.com/Calendar.aspx" --jurisdiction "County of Sonoma" --body-filter "Board of Supervisors" --meeting-limit 200 --headless --minutes-db minutes.db --form700-folder form700 --out votes.csv
```

Live interval run:

```bash
python -m civic_vote_scraper.cli --url "https://sonoma-county.legistar.com/Calendar.aspx" --jurisdiction "County of Sonoma" --body-filter "Board of Supervisors" --meeting-limit 200 --headless --minutes-db minutes.db --form700-folder form700 --live --live-interval-minutes 60 --out votes.csv
```

Useful options:

```bash
--form700-search-url "https://form700search.fppc.ca.gov/Search/SearchFilerForms.aspx"
--reparse-existing-form700s
--reparse-existing-minutes
--skip-form700-sync
```

Form 700 outputs are written from the database-backed PDF parse:

- `form700_entities.csv`
- `form700_entities.json`
- `form700_matches.csv`
- `form700_matches.json`

The desktop app exposes the same flow with:

- `Minutes database`: where discovered minutes and parsed vote rows are stored.
- `Form 700 search URL`: the FPPC portal entrypoint.
- `Form 700 folder`: where downloaded filing PDFs are stored.
- `Search interval (minutes)`: how often live search checks for new minutes.
- `Start live search`: starts the repeating scrape.
- `Run once`: performs one database-backed scrape cycle.
- `Re-parse known minutes`: forces already parsed database records to be parsed again.
- `Re-parse known Form 700 PDFs`: re-runs PDF parsing for saved filings.


## Known Issues and Limitations

- Minutes file matching is not 100% work
- Can only pull from Legistar websites

