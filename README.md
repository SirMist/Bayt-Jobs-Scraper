[Bayt Jobs Scraper](https://apify.com/shahidirfan/Bayt-Jobs-Scraper?fpr=data)

Extract high-coverage Bayt job data in a structured dataset, including listing insights, job descriptions, skills, and preferred candidate details. This actor is built for teams that need reliable job market datasets for hiring analysis, lead generation, and trend monitoring. It supports country pages, keyword pages, and filtered search URLs.

## Features

- **Broad Job Coverage** — Collect jobs from international, country, city, and keyword listing pages.
- **Richer Job Records** — Capture title, company, career level, experience hints, skills, and preferred candidate data.
- **Pagination Support** — Automatically walks through result pages until your target count is reached.
- **Flexible Collection Size** — Scrape a fixed number of jobs or gather all available jobs.
- **Proxy Ready** — Works with proxy configuration for stable large runs.

## Use Cases

### Recruitment Intelligence

Build fresh hiring datasets by role, region, and career level. Track demand changes and identify where employers are actively hiring.

### Sales Prospecting

Discover companies posting new roles and enrich outreach lists with job context, location, and company profile links.

### Labor Market Research

Analyze skills demand, employment types, and role distribution across markets for quarterly or monthly reports.

### HR Benchmarking

Compare job requirements and role positioning across competitors to improve your own hiring strategy.

---

## Input Parameters

| Parameter | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| `startUrl` | String | No | `https://www.bayt.com/en/international/jobs/` | Listing page to start from (country, city, keyword, or filtered URL). |
| `results_wanted` | Integer | No | `20` | Maximum number of jobs to collect. Leave empty to collect all available jobs. |
| `max_pages` | Integer | No | `20` | Maximum listing pages to visit during the run. |
| `proxyConfiguration` | Object | No | `{ "useApifyProxy": false }` | Proxy settings for the run. |

---

## Output Data

Each dataset item includes:

| Field | Type | Description |
| --- | --- | --- |
| `source` | String | Data source (`bayt.com`). |
| `jobId` | String | Bayt job identifier. |
| `url` | String | Job posting URL. |
| `title` | String | Job title. |
| `company` | String | Employer name. |
| `companyProfileUrl` | String | Company profile link. |
| `location` | String | Job location text. |
| `postedAtRelative` | String | Relative posting time (for example, `2 days ago`). |
| `employmentType` | String | Employment type, when available. |
| `careerLevel` | String | Career level. |
| `experience` | String | Experience requirement snippet, when available. |
| `salary` | String | Salary snippet, when available. |
| `industryId` | String/Number | Industry identifier from listing metadata. |
| `jobType` | String | Job type flag from listing metadata. |
| `applyUrl` | String | Apply link for the job. |
| `isExternalJob` | Boolean | Indicates if the application is external. |
| `hasApplied` | Boolean/String | Application status indicator. |
| `jobIsActive` | Boolean | Job active status indicator. |
| `companyStatus` | Number | Company status flag. |
| `descriptionHtml` | String | Job description HTML. |
| `descriptionText` | String | Clean text version of the description. |
| `skillsHtml` | String | Skills section HTML. |
| `skillsText` | String | Clean text version of skills section. |
| `preferredCandidate` | Object | Preferred candidate fields (when provided). |
| `listingSummary` | String | Listing card summary snippet. |
| `keyword` | String | Active keyword in the listing filters. |
| `locationFilter` | String | Active location filter payload. |
| `listingPage` | Number | Listing page number where job was found. |
| `listingUrl` | String | Listing URL where job was collected. |
| `rawJobPost` | Object | Raw listing-level job data object. |
| `rawMiniJobData` | Object | Raw extra job metadata object. |
| `scrapedAt` | String | ISO timestamp of extraction. |

---

## Usage Examples

### Default Run

```
{
  "startUrl": "https://www.bayt.com/en/international/jobs/",
  "results_wanted": 20
}
```

### Country-Specific Collection

```
{
  "startUrl": "https://www.bayt.com/en/uae/jobs/",
  "results_wanted": 100,
  "max_pages": 10
}
```

### Keyword-Driven Collection

```
{
  "startUrl": "https://www.bayt.com/en/international/jobs/software-engineer-jobs/",
  "results_wanted": 150,
  "max_pages": 20
}
```

---

## Sample Output

```
{
  "source": "bayt.com",
  "jobId": "5441109",
  "url": "https://www.bayt.com/en/uae/jobs/hotel-sales-manager-5441109/",
  "title": "Hotel Sales Manager",
  "company": "Taurus Recruitment",
  "companyProfileUrl": "https://www.bayt.com/en/company/taurus-recruitment-2255833/",
  "location": "Abu Dhabi · UAE",
  "postedAtRelative": "14 minutes ago",
  "employmentType": "Full time",
  "careerLevel": "Management",
  "experience": null,
  "salary": null,
  "industryId": "700",
  "jobType": "native",
  "applyUrl": "https://www.bayt.com/en/job/apply/index/5441109/",
  "isExternalJob": false,
  "hasApplied": "0",
  "jobIsActive": true,
  "companyStatus": 0,
  "descriptionText": "Job Title: Hotel Sales Manager Location: Abu Dhabi (UAE)...",
  "skillsText": "Commercial & Revenue Skills ...",
  "preferredCandidate": {
    "career_level": "Management"
  },
  "listingSummary": "Job Title: Hotel Sales Manager Location: Abu Dhabi (UAE)...",
  "keyword": null,
  "locationFilter": "[]",
  "listingPage": 1,
  "listingUrl": "https://www.bayt.com/en/international/jobs/",
  "scrapedAt": "2026-03-02T08:10:00.000Z"
}
```

---

## Tips For Best Results

### Start With Focused URLs

- Use country or city listing URLs for targeted datasets.
- Use keyword listing URLs for role-specific research.

### Control Runtime

- Keep `results_wanted` around `20-100` for quick validation runs.
- Increase `max_pages` only when you need deeper coverage.

### Improve Stability

- Keep proxy enabled for medium to large runs.

---

## Integrations

Connect your dataset with:

- **Google Sheets** — Share and analyze with teams.
- **Airtable** — Build searchable recruiting datasets.
- **Slack** — Push hiring alerts to channels.
- **Make** — Automate downstream workflows.
- **Zapier** — Trigger actions in your stack.
- **Webhooks** — Deliver data to custom systems.

### Export Formats

- **JSON** — For developers and automation pipelines.
- **CSV** — For spreadsheet analysis.
- **Excel** — For business reporting.
- **XML** — For system integrations.

---

## Frequently Asked Questions

### How many jobs can I collect in one run?

You can collect as many as available, limited by your input settings and run environment.

### Can I scrape specific countries or cities?

Yes. Set `startUrl` to the relevant country or city jobs page.

### Can I collect only specific roles?

Yes. Use keyword-specific listing URLs in `startUrl`.

### Why are some fields empty?

Some jobs do not expose all metadata. Empty values mean the source listing did not provide that field.

### How do I speed up test runs?

Use `results_wanted: 20`, keep `max_pages` low, and start with a focused listing URL.

---

## Support

For issues or feature requests, use the Apify Console run details and logs.

### Resources

- [Apify Documentation](https://docs.apify.com/)
- [Apify API Reference](https://docs.apify.com/api/v2)
- [Apify Schedules](https://docs.apify.com/platform/schedules)

---

## Legal Notice

This actor is intended for lawful data collection and analysis. You are responsible for complying with Bayt terms, local laws, and applicable data-use requirements.