# PyPI Scraper: export to CSV or JSON with Python, Node.js or curl

Snapshot PyPI package metadata using the public pypi.org/pypi/<pkg>/json endpoint. One row per package with latestVersion, summary, license, classifiers, requiresPython, project URLs, release count.

This repository has ready scripts for the [PyPI Scraper: Python Package Details and Stats](https://apify.com/v0iddo/pypi-package-tracker) Actor on Apify. The Actor does the
fetching, paging and cleaning; the scripts here run it with your input and save the results as a CSV or JSON file.

## Quick start

1. Create an Apify account and copy your API token from
   [Console > Settings > Integrations](https://console.apify.com/settings/integrations).
2. Install the client and run the script with the example input:

```bash
pip install "apify-client>=2"
export APIFY_TOKEN=your_token
python run.py input.json -o results.csv
```

Edit `input.json` for your own run. You can also run the Actor in the browser on its
[Apify page](https://apify.com/v0iddo/pypi-package-tracker), schedule it there, and connect its output to Google Sheets, Slack, Zapier or Make.

## Input

The example input (`input.json`):

```json
{
  "packages": [
    "requests",
    "numpy",
    "pandas"
  ]
}
```

| Field | Type | What it does |
| --- | --- | --- |
| `packages` | array | PyPI package names |

## Output

One row per result. A real row from a run of the example input (more in
[`examples/sample-output.json`](examples/sample-output.json)):

```json
{
  "snapshotAt": "2026-11-21T02:40:54+00:00",
  "name": "requests",
  "latestVersion": "2.34.2",
  "summary": "Python HTTP for Humans.",
  "license": "Apache-2.0",
  "homepage": null,
  "projectUrls": {
    "Documentation": "https://requests.readthedocs.io",
    "Source": "https://github.com/psf/requests"
  },
  "requiresPython": ">=3.10",
  "classifiers": [
    "Development Status :: 5 - Production/Stable",
    "Environment :: Web Environment",
    "Intended Audience :: Developers",
    "License :: OSI Approved :: Apache Software License",
    "Natural Language :: English"
  ],
  "keywords": null,
  "releaseCount": 163
}
```

Fields: `snapshotAt`, `name`, `latestVersion`, `summary`, `license`, `homepage`, `projectUrls`, `requiresPython`, `classifiers`, `keywords`, `releaseCount`.

## Node.js

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('v0iddo/pypi-package-tracker').call({"packages": ["requests", "numpy", "pandas"]});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
```

Full example: [`examples/node/run.mjs`](examples/node/run.mjs).

## Any language: one HTTP request

```bash
curl -X POST "https://api.apify.com/v2/acts/v0iddo~pypi-package-tracker/run-sync-get-dataset-items" \
  -H "Authorization: Bearer $APIFY_TOKEN" -H 'Content-Type: application/json' \
  -d '{"packages": ["requests", "numpy", "pandas"]}'
```

The response is the JSON array of results.

## Cost

The Actor is paid per result: **$0.20 per 1,000 results**, nothing per run. A new Apify account gets $5 of platform credit every month.

## License

MIT for the code in this repository. Made by [vøiddo](https://voiddo.com).
