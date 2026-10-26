# SOC Quarantine Mail Template Generator

A single-file, browser-based tool for SOC analysts that turns exported Microsoft Defender quarantine results into ready-to-paste incident tickets (Jira-style **Incident Description** and **Incident Analysis**).

Everything runs locally in the browser. No data is uploaded, no backend, no dependencies to install.

## Features

- **KQL builder**: pick a start and end date and the Advanced Hunting query updates automatically. Includes quick presets (Today, Yesterday, Last 2 days, Last 7 days) and a one-click **Copy KQL** button.
- **CSV file picker**: load the exported query results by clicking or drag and drop. Handles quoted fields, BOM and the `TimeReceived [Local Time]` header.
- **Section selector**: the query splits results into 4 sets (`SetNumber` 1-4). Choose a section from the dropdown and only that section is parsed into the template.
- **Template generation**: Incident Description (defanged sender addresses) and Incident Analysis (sender, time, IP, subject, reputation, observation, quarantine reason, detection technologies).
- **Defang tool**: converts domains, URLs and IPs to a safe format (`hxxp`, `[.]`).
- **Light and dark mode**: follows the system setting, with a manual toggle that is remembered.
- **Copy to clipboard**: output is copied as Jira-friendly markup (`**bold**` and newlines).

## How to use

1. **Build the query**: choose the date range in step 1, click **Copy KQL**, and run it in Microsoft Defender XDR (Advanced Hunting) or Microsoft Sentinel.
2. **Export** the results as CSV.
3. **Load the CSV** in step 2 and pick a section from the dropdown.
4. **Copy** the Incident Description and Incident Analysis into your ticket, then fill in the `Observation:` line.
5. Use the **Defang tool** for any extra domains, URLs or IPs.

## Expected CSV columns

| Column | Notes |
|---|---|
| `SetNumber` | 1-4, used for the section dropdown |
| `Recipient` | Recipient mailbox |
| `SenderAddress` | Sender from-address |
| `SenderIP` | IPv4 or IPv6 |
| `TimeReceived` | Also accepted as `TimeReceived [Local Time]` |
| `SubjectLine` | Email subject |
| `QuarantineReason` | Policy or threat type |
| `DetectionTechnology` | JSON, flattened into `Phish: ... \| Spam: ...` |

## Run locally

Open `index.html` in any modern browser.

## Deploy with GitHub Pages

1. Push `index.html` to the repository root.
2. Go to **Settings > Pages**, choose **Deploy from a branch**, select `main` and `/ (root)`.
3. Your tool is live at `https://<username>.github.io/<repo-name>/`.

## Customising the query

The recipient list and the quarantine filters live in the `KQL` template near the top of the script in `index.html`. Replace the example mailboxes with your own monitored recipients. Only the two `datetime(...)` values are generated from the date pickers.

```kql
let Recipients = dynamic([
    "user1@example.com",
    "user2@example.com"
]);
```

## Privacy and data handling

- Parsing happens entirely in your browser.
- Do **not** commit exported CSV files or real email data to the repository (the included `.gitignore` excludes `*.csv`).
- Use placeholder recipients and domains in any public fork.

## Tech

Plain HTML, CSS and vanilla JavaScript in one file.

## Author

Built by **Thondarock** for efficient SOC incident reporting.
