# Shares Outstanding (SEC Company Concepts) — Web App

## Overview
This lightweight web app fetches the SEC Company Concepts API for the metric:
- dei: EntityCommonStockSharesOutstanding

It computes the maximum and minimum shares outstanding for fiscal years greater than 2020 and displays the results clearly.

Key points:
- Defaults to Chipotle Mexican Grill (CIK 0001058090).
- Dynamically supports any other 10-digit CIK via a query parameter, using a proxy (e.g., AIPipe r.jina.ai) to avoid CORS and header constraints.
- Provides a one-click “Download data.json” snapshot matching the requested structure.

DOM element IDs required by the brief are updated live:
- share-entity-name
- share-max-value, share-max-fy
- share-min-value, share-min-fy

The page title and H1 include the live entity name.

Note: Browsers cannot set a custom User-Agent header; the app attempts a direct fetch to the SEC API for the default CIK and falls back to a proxy as needed. When a CIK is provided via the query string or the UI, the app prefers the proxy per the instructions.

## Setup
No build steps required.

1. Save the provided index.html file.
2. Open index.html in any modern browser with internet access.

Optional: Host on any static server (GitHub Pages, Netlify, Vercel, etc.).

## Usage
- Default load:
  - Open index.html to fetch Chipotle Mexican Grill data from the SEC API. The app filters for FY > 2020 and shows the min/max shares.
- Load a different company (proxy path):
  - Append a 10-digit CIK: index.html?CIK=0001018724 (example: Amazon).
  - Alternatively, type a 10-digit CIK in the input and click “Load”. This uses a proxy and updates the page without reloading.
- Download snapshot:
  - Click “Download data.json” to download a JSON file shaped exactly as required:
    {
      "entityName": "…",
      "max": { "val": number, "fy": "string" },
      "min": { "val": number, "fy": "string" }
    }

Filtering logic:
- Uses json.units.shares entries only.
- Keeps entries with numeric val and fy where fy > 2020.
- Computes min and max by val (ties broken by first occurrence).

Data source:
- SEC Company Concepts API:
  https://data.sec.gov/api/xbrl/companyconcept/CIK{CIK}/dei/EntityCommonStockSharesOutstanding.json

Proxy used when needed:
- https://r.jina.ai/https://data.sec.gov/api/xbrl/companyconcept/…

Compliance:
- The app includes a descriptive identifier header where possible. Actual User-Agent headers cannot be set by browsers; the proxy path helps maintain accessibility.

## Round
1