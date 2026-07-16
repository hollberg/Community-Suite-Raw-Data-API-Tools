# Step 1: Get Your API Credentials from CSuite

Before you can connect Power BI or Excel to CSuite, you need to turn on the API for a custom report and collect **four values**. This takes about five minutes and you only do it once per report.

You'll collect:

| Value | What it looks like | Notes |
|-------|--------------------|-------|
| **Your organization's URL prefix** | `yourorg` (as in `https://yourorg.fcsuite.com`) | The first part of the web address you use to log in to CSuite. This never changes for your organization. |
| **report_id** | `1234` | A number identifying the specific custom report. |
| **SIGNER** | `abcd...wxyz` (long code) | An access key. Tied to *your* CSuite user, so it's the same for every report you enable. |
| **SIGNATURE** | `AAAA...zzz=` (long code) | An access key specific to the report. |

## Before you start

- You need a CSuite login that can view the **Reports** page and enable a report's API. If you don't see the options described below, ask your CSuite administrator about your permissions.
- You need a saved **custom report** containing the data you want. Any custom report works — if you can run it in CSuite, you can connect it.

> [!TIP]
> **Best practice:** create a report category like `zAdmin – API Reports` and assign it to every report you API-enable. Anyone editing a CSuite report changes the data flowing into every connected Power BI/Excel file — a clearly labeled category tells your colleagues "this report feeds a dashboard, edit with care."

## Enable the API on your report

1. In CSuite, go to the **Reports** page.
2. Find your custom report in the list. Each report has an **API** column.
3. Click **Enable** in the API column next to your report.

   <!-- TODO: screenshot of the Reports page showing the API column and Enable link -->

4. Once enabled, the link changes to **Show Key**. Click **Show Key** to open the report's API configuration screen.

   <!-- TODO: screenshot of the Show Key link -->

## Copy your four values

The API configuration screen shows **two sets** of connector values — one labeled for the **Power BI Connector** and one for the **Raw API**. The templates in this repository use the **Raw API** values.

From the **Raw API** section, copy:

- **url** — for example `https://yourorg.fcsuite.com/api/v1/report/display`. You only need the prefix (the `yourorg` part) for the templates.
- **report_id** — the report's number.
- **SIGNER** — long access key.
- **SIGNATURE** — long access key.

<!-- TODO: screenshot of the API configuration screen with the Raw API values highlighted (keys blurred!) -->

Paste them somewhere safe and temporary (see the warning below) — the next guide will have you paste them into Power BI or Excel.

> [!NOTE]
> Copying keys from the Power BI Connector section by mistake is a common trip-up — the **Key ID** and **Token** there are *not* the same as SIGNER and SIGNATURE, and the templates won't work with them.

## Keep your keys safe

> [!WARNING]
> **Treat SIGNER and SIGNATURE like passwords.** Anyone who has them can pull this report's data from anywhere on the internet — no CSuite login required. The Raw API has no additional security layer beyond these keys.

Practical rules of thumb:

- **Don't** email the keys, paste them into chat, or store them in shared documents.
- **Don't** commit files containing keys to a shared repository (like GitHub). Keep the placeholder versions in the repo; keep your filled-in versions in your own workbooks/reports only.
- **Do** think before enabling the API on sensitive reports (donor giving history, staff data, etc.). Once shared, a Power BI file or Excel workbook contains the keys inside its query.
- **Do** remember that your **SIGNER** is tied to *you*. If you leave the organization or your keys are exposed, your foundation should disable and re-enable the affected report APIs (or contact [Foundant Support](mailto:support@foundant.com)) to issue fresh keys.

## Next step

Now connect your tool of choice:

- **[Power BI step-by-step guide →](powerbi-guide.md)**
- **[Excel step-by-step guide →](excel-guide.md)**
