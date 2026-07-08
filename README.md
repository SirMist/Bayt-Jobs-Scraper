[Bayt Jobs Scraper](https://apify.com/fatihtahta/bayt-jobs-scraper?fpr=data)

# Bayt Jobs Scraper

**Slug:** `fatihtahta/bayt-jobs-scraper`

## Overview

Bayt Jobs Scraper collects structured job listing data from [Bayt](https://www.bayt.com), including job titles, descriptions, companies, locations, salary details, experience requirements, application attributes, and source metadata. It is designed for teams that need consistent job-market data without manually reviewing listings one by one. [Bayt](https://www.bayt.com) is a widely used employment platform across the Middle East and nearby markets, making it a practical source for hiring, compensation, and labor-market analysis. The actor automates recurring collection workflows so you can capture fresh listings in a repeatable format. This saves time, reduces manual effort, and makes downstream reporting and integration easier.

## Why Use This Actor

- **Market research and analytics teams:** Track hiring demand, salary ranges, locations, and seniority trends across roles, regions, and time periods.
- **Product and content teams:** Identify in-demand job titles, skills, and employer patterns to inform editorial calendars, landing pages, and market-facing content.
- **Developers and data engineering teams:** Feed structured Bayt job data into ETL pipelines, internal dashboards, search indexes, and downstream APIs.
- **Lead generation and enrichment teams:** Discover active hiring companies, enrich account records, and prioritize outreach based on live recruitment activity.
- **Monitoring and competitive intelligence teams:** Watch changes in posting volume, target geographies, role mix, and hiring signals for selected markets or segments.

## Input Parameters

Provide any combination of URLs, queries, and filters. If you leave both `startUrls` and `queries` empty, the actor starts from `https://www.bayt.com/en/international/jobs/`.

| Parameter | Type | Description | Default |
| --- | --- | --- | --- |
| `enrichData` | `boolean` | Collect fuller job details when available. Leave off for faster, lighter runs. | `false` |
| `queries` | `string[]` | One or more free-text searches such as job titles, keywords, skills, or role names. Each entry runs as a separate search. Ignored when `startUrls` is provided. | – |
| `location` | `string` | Country or city filter. Allowed values: `algeria`, `bahrain`, `bahrain/manama`, `egypt`, `egypt/alexandria`, `egypt/cairo`, `india`, `iraq`, `iraq/baghdad`, `jordan`, `jordan/amman`, `kuwait`, `kuwait/al-kuwait`, `lebanon`, `lebanon/beirut`, `libya`, `morocco`, `morocco/casablanca`, `oman`, `oman/muscat`, `pakistan`, `qatar`, `qatar/doha`, `saudi-arabia`, `saudi-arabia/dammam`, `saudi-arabia/eastern-province`, `saudi-arabia/jeddah`, `saudi-arabia/jubail`, `saudi-arabia/khobar`, `saudi-arabia/mecca`, `saudi-arabia/medina`, `saudi-arabia/riyadh`, `syria`, `tunisia`, `tunisia/tunis`, `uae`, `uae/abu-dhabi`, `uae/ajman`, `uae/al-ain`, `uae/dubai`, `uae/fujairah`, `uae/ras-al-khaimah`, `uae/sharjah`, `yemen`. | – |
| `datePosted` | `string` | Posting recency filter. Allowed values: `1` = Posted in last 30 days, `2` = Posted in last 7 days, `3` = Posted in last 24 hours. | – |
| `experience_level` | `string[]` | Seniority filter. Allowed values: `0` = Student/Internship, `6` = Fresh graduate, `1` = Entry level, `2` = Mid career, `3` = Management, `4` = Director/Head, `5` = Senior executive. | – |
| `gender` | `string[]` | Gender requirement filter for listings that specify it. Allowed values: `0` = No Gender requirements, `1` = Only Male, `2` = Only Female. | – |
| `remoteOnly` | `boolean` | Include only jobs marked as remote. | `false` |
| `easyApply` | `boolean` | Include only jobs that support Bayt Easy Apply. | `false` |
| `fewerApplicants` | `boolean` | Include only jobs marked as having fewer applicants. | `false` |
| `startUrls` | `string[]` | One or more Bayt job pages or Bayt search result URLs to scrape directly. When provided, these URLs override query-based seed building. | – |
| `limit` | `integer` | Maximum number of jobs to save per search or URL. Minimum value: `1`. Leave empty to collect all available matching results. | – |
| `proxyConfiguration` | `object` | Optional Apify or custom proxy settings. | Apify proxy with `RESIDENTIAL` group |

## Example Inputs

### Scenario: Search-driven run

```
{
  "queries": ["software engineer", "backend developer"],
  "location": "uae/dubai",
  "datePosted": "2",
  "experience_level": ["1", "2"],
  "enrichData": true,
  "limit": 150
}
```

### Scenario: Direct URL run

```
{
  "startUrls": [
    "https://www.bayt.com/en/saudi-arabia/jobs/data-analyst-jobs/",
    "https://www.bayt.com/en/uae/jobs/remote-jobs/"
  ],
  "enrichData": false,
  "easyApply": true,
  "fewerApplicants": true,
  "limit": 100
}
```

### Scenario: Filtered targeted run

```
{
  "queries": ["customer support"],
  "location": "qatar/doha",
  "datePosted": "3",
  "gender": ["0"],
  "remoteOnly": true,
  "easyApply": true,
  "limit": 50
}
```

## Output

### 6.1 Output destination

The actor writes results to an Apify dataset as JSON records. And the dataset is designed for direct consumption by analytics tools, ETL pipelines, and downstream APIs without post-processing.

### 6.2 Record envelope (all items)

Every record includes these stable identifiers:

- **type** *(string, required)*: Record type. Current value: `job`.
- **id** *(number, required)*: Numeric Bayt job identifier.
- **url** *(string, required)*: Canonical Bayt job URL.

Recommended idempotency key: `type + ":" + id`

If the same listing is discovered through multiple searches or source URLs, use `type + ":" + id` for deduplication and upserts.

### 6.3 Examples

Example: job (`type = "job"`)

```
{
  "type": "job",
  "id": 5442853,
  "url": "https://www.bayt.com/en/saudi-arabia/jobs/%D9%85%D8%A8%D8%B1%D9%85%D8%AC-%D8%AF%D8%B1%D9%88%D8%A8%D8%A7%D9%84-drupal7-php-5442853/",
  "canonicalUrl": "https://www.bayt.com/en/saudi-arabia/jobs/%D9%85%D8%A8%D8%B1%D9%85%D8%AC-%D8%AF%D8%B1%D9%88%D8%A8%D8%A7%D9%84-drupal7-php-5442853/",
  "sourceUrl": "https://www.bayt.com/en/international/jobs/software-engineer-jobs/",
  "title": "Drupal 7 PHP Developer",
  "description": "The organization seeks a PHP Developer to work within the technical team, responsible for developing and maintaining digital platforms built on Drupal 7.",
  "company": "Confidential Company",
  "location": "Jeddah · Saudi Arabia",
  "companyLocation": "Jeddah · Saudi Arabia",
  "companyLogoUrl": "https://secure.b8cdn.com/bayt/assets/jobs-84tl0qsn/images/no-logo.svg",
  "price": "SAR 5,769 - SAR 7,692",
  "priceNumeric": 5769,
  "currency": "SAR",
  "salaryText": "SAR 5,769 - SAR 7,692",
  "salaryMin": 5769,
  "salaryCurrency": "SAR",
  "industry": "Market Research",
  "careerLevel": "Mid career",
  "employmentType": "Full time",
  "jobType": "native",
  "yearsOfExperience": "2+ years",
  "degree": "Bachelor's degree / higher diploma",
  "skills": [
    "Required Skills",
    "Proficiency in PHP with experience in Drupal 7 or a genuine willingness to learn it",
    "Knowledge of MySQL and Git and fundamentals of HTML / CSS / JS"
  ],
  "companyDescription": "Large diversified employer profile text, when available on the listing.",
  "companyProfileUrl": "https://www.bayt.com/en/company/al-futtaim-group-1661592/",
  "applyUrl": "https://www.bayt.com/en/job/apply/index/5442853/",
  "easyApply": true,
  "hasQuestionnaire": false,
  "jobStatusCode": 2,
  "postedText": "19 days ago",
  "expiresIn": "Expires in 15 days",
  "translatedByAI": true,
  "originalLanguageAvailable": true,
  "translationDisclaimer": "This job post has been translated by AI and may contain minor differences or errors.",
  "preferredCandidate": {
    "Years of experience": "2+ years",
    "Degree": "Bachelor's degree / higher diploma",
    "Career level": "Mid career"
  },
  "domain": "bayt.com",
  "seedId": "1903bae84e70",
  "seedType": "url",
  "seedValue": "https://www.bayt.com/en/international/jobs/software-engineer-jobs/",
  "pageIndex": 1,
  "fingerprint": "71652b7b32bff16a110c"
}
```

## Field reference

### Job fields (`type = "job"`)

- **type** *(string, required)*: Record type. Current value: `job`.
- **id** *(number, required)*: Numeric Bayt job identifier.
- **url** *(string, required)*: Canonical job URL.
- **canonicalUrl** *(string, optional)*: Canonicalized Bayt URL for the listing.
- **sourceUrl** *(string, optional)*: Input URL or originating search page.
- **title** *(string, required)*: Job title.
- **description** *(string, optional)*: Listing description text.
- **company** *(string, optional)*: Employer name.
- **location** *(string, optional)*: Listing location text.
- **companyLocation** *(string, optional)*: Employer location text when separately available.
- **companyLogoUrl** *(string, optional)*: Company logo URL.
- **price** *(string, optional)*: Salary text as displayed.
- **priceNumeric** *(number, optional)*: Numeric salary convenience value when available.
- **currency** *(string, optional)*: Currency code for salary-related fields.
- **salaryText** *(string, optional)*: Salary range text.
- **salaryMin** *(number, optional)*: Lower bound of salary when available.
- **salaryCurrency** *(string, optional)*: Currency for normalized salary fields.
- **industry** *(string, optional)*: Employer industry or category.
- **careerLevel** *(string, optional)*: Seniority or career level.
- **employmentType** *(string, optional)*: Employment type such as full time.
- **jobType** *(string, optional)*: Listing type label.
- **yearsOfExperience** *(string, optional)*: Experience requirement text.
- **degree** *(string, optional)*: Education requirement text.
- **skills** *(array[string], optional)*: Skills or requirements listed on the job.
- **companyDescription** *(string, optional)*: Employer description text.
- **companyProfileUrl** *(string, optional)*: Public Bayt company profile URL.
- **applyUrl** *(string, optional)*: Direct Bayt apply URL for the listing when shown.
- **easyApply** *(boolean, optional)*: Whether the listing supports Bayt Easy Apply.
- **hasQuestionnaire** *(boolean, optional)*: Whether Bayt marks the job as having a questionnaire.
- **jobStatusCode** *(number, optional)*: Raw Bayt job status code from the detail payload.
- **postedText** *(string, optional)*: Relative posting date text.
- **expiresIn** *(string, optional)*: Relative expiry text.
- **translatedByAI** *(boolean, optional)*: Whether the listing detail is shown as AI-translated by Bayt.
- **originalLanguageAvailable** *(boolean, optional)*: Whether the detail view exposes a link to the original language.
- **translationDisclaimer** *(string, optional)*: Translation warning text shown on AI-translated listings.
- **preferredCandidate** *(object[string,string], optional)*: Label/value pairs collected from the Preferred candidate section.
- **domain** *(string, optional)*: Source domain.
- **seedId** *(string, optional)*: Internal seed identifier for the input source.
- **seedType** *(string, optional)*: Source input type, such as `url`.
- **seedValue** *(string, optional)*: Original query or URL value that led to the record.
- **pageIndex** *(number, optional)*: Result page number where the record was found.
- **fingerprint** *(string, optional)*: Record fingerprint for change tracking or dedup workflows.

## Data guarantees & handling

- **Best-effort extraction:** fields may vary by region, session, availability, or UI experiments.
- **Optional fields:** null-check in downstream code.
- **Deduplication:** recommend `type + ":" + id`.

## How to Run on Apify

1. Open the actor in Apify Console.
2. Configure your search parameters, such as keywords, location, posted date, and optional filters.
3. Set the maximum number of outputs to collect.
4. Click **Start** and wait for the run to finish.
5. Download results in JSON, CSV, Excel, or another supported format.

## Scheduling & Automation

### Scheduling

**Automated Data Collection**

You can schedule recurring runs to keep your job dataset fresh without manually starting the actor each time. This is useful for ongoing market monitoring, reporting, and automated downstream processing.

- Navigate to **Schedules** in Apify Console.
- Create a new schedule (daily, weekly, or custom cron).
- Configure input parameters.
- Enable notifications for run completion.
- Optional: add webhooks for automated processing.

### Integration Options

- **Webhooks:** Trigger downstream actions when a run completes.
- **Zapier:** Connect to 5,000+ apps without coding.
- **Make (Integromat):** Build multi-step automation workflows.
- **Google Sheets:** Export results to a spreadsheet.
- **Slack/Discord:** Receive notifications and summaries.
- **Email:** Send automated reports via email.

## Performance

Estimated run times:

- **Small runs (< 1,000 outputs):** ~2–3 minutes
- **Medium runs (1,000–5,000 outputs):** ~5–15 minutes
- **Large runs (5,000+ outputs):** ~15–30 minutes

Execution time varies based on filters, result volume, and how much information is returned per record.

## Compliance & Ethics

### Responsible Data Collection

This actor collects publicly available job listing information from [https://www.bayt.com](https://www.bayt.com) for legitimate business purposes, including labor market research and market analysis, recruiting intelligence, and competitive monitoring. Users are responsible for ensuring their collection and use of data complies with applicable laws, regulations, and platform terms. This section is informational and not legal advice.

### Best Practices

- Use collected data in accordance with applicable laws, regulations, and the target site's terms.
- Respect individual privacy and personal information.
- Use data responsibly and avoid disruptive or excessive collection.
- Do not use this actor for spamming, harassment, or other harmful purposes.
- Follow relevant data protection requirements where applicable, including GDPR and CCPA.

## Support

For help, use the actor page or the Issues tab in Apify Console. When reporting a problem, include the input you used with sensitive values redacted, the run ID, a short note on expected versus actual behavior, and an optional small output sample that shows the issue.