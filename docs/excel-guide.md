# Step 2 (Excel): Connect Excel to Your CSuite Report

This guide walks you through pulling a CSuite custom report into an **Excel workbook** using [ExcelTemplate.txt](../ExcelTemplate.txt). Once set up, refreshing the workbook pulls the latest data straight from CSuite — no more exporting CSVs.

**Before you start**, you need:

- Your four values from CSuite — if you don't have them yet, do **[Step 1: Get your API credentials](getting-your-api-credentials.md)** first.
- **Desktop Excel** (Windows or Mac). Power Query is not available in Excel for the web.

Setup takes about 10 minutes.

## Part 1: Create the query

1. Open a new (or existing) Excel workbook.
2. On the **Data** ribbon, click **Get Data** → **From Other Sources** → **Blank Query**. The Power Query Editor window opens.

   <!-- TODO: screenshot of Data > Get Data > From Other Sources > Blank Query -->

3. In the Power Query Editor, click **Advanced Editor** on the Home ribbon. A window opens showing a small amount of placeholder code.
4. Open [ExcelTemplate.txt](../ExcelTemplate.txt) (click the link, then use the copy button on GitHub), select **all** of its text, and paste it into the Advanced Editor — **replacing everything** that was there.

   <!-- TODO: screenshot of the Advanced Editor with the template pasted in -->

## Part 2: Fill in your four values

In the pasted code, find and replace these four placeholders with your values from Step 1. Replace the *entire* placeholder **including the angle brackets and asterisks**, but keep the surrounding quotation marks:

| Placeholder | Replace with | Example result |
|-------------|--------------|----------------|
| `<**ORG_PREFIX_HERE**>` | Your organization's URL prefix | `"https://yourorg.fcsuite.com/api/v1/report/display"` |
| `<**SIGNER**>` | Your SIGNER key | `#"SIGNER" = "abcd...wxyz"` |
| `<**SIGNATURE**>` | The report's SIGNATURE key | `#"SIGNATURE" = "AAAA...zzz="` |
| `<**REPORT_ID**>` | The report_id number | `"report_id=1234"` |

5. Click **Done**. If the Advanced Editor shows a syntax error at the bottom, double-check that all quotation marks are still in place around your values.
6. If Excel asks how to connect, choose **Anonymous** access and click **Connect**. (This is correct — your real credentials are the SIGNER/SIGNATURE inside the query itself.)
7. Your report data should now appear as a table in the Power Query Editor. 🎉
8. Give the query a meaningful name (right-hand pane → **Name**) — use the CSuite report name so future-you knows where the data comes from.
9. Click **Close & Load** on the Home ribbon. The data lands in a new worksheet as a formatted table.

To connect **additional CSuite reports** in the same workbook, repeat these steps — each report has its own `report_id` and SIGNATURE (your org prefix and SIGNER stay the same).

## Refreshing your data

- **Refresh everything:** Data ribbon → **Refresh All**.
- **Refresh one table:** right-click anywhere in the table → **Refresh**.

Every refresh re-runs the CSuite report and replaces the table contents with current data. Formulas, pivot tables, and charts pointing at the table update automatically.

> [!TIP]
> **Leave breadcrumbs.** Six months from now, nobody will remember which CSuite report feeds this workbook. Add a note in the workbook (or in the query name) with the CSuite report **name**, its **report_id**, and a link back to the report, e.g. `https://YOUR-PREFIX.fcsuite.com/erp/report/display?report_id=1234&action=load`.

## Troubleshooting

| Symptom | Likely cause and fix |
|---------|---------------------|
| **403 / Forbidden or credentials error** | A SIGNER, SIGNATURE, or report_id value was mistyped, or you copied the **Power BI Connector** Key ID/Token instead of the **Raw API** SIGNER/SIGNATURE. Re-copy from CSuite (Reports page → **Show Key**). |
| **Error mentioning a rejected timestamp** | Your computer's clock is too far off — the API rejects requests with bad timestamps. Correct your PC's clock and try again. |
| **Expression syntax error after pasting** | A quotation mark or comma was accidentally deleted while replacing placeholders. Re-paste the template and try again carefully. |
| **Can't find Get Data → Blank Query** | You may be in Excel for the web, or a very old Excel version. Use desktop Excel (2016 or later; Microsoft 365 recommended). |
| **Columns or data changed unexpectedly** | Someone edited the custom report in CSuite — filters, columns, and sorting there flow straight through to Excel. See the [best practice](getting-your-api-credentials.md#before-you-start) of flagging API reports with a special category. |
| **A column shows values mashed together with commas** | That's a CSuite "Array" (list) field — the template converts lists to comma-separated text so the query doesn't fail. If you need those items separated, use Power Query's **Split Column**, or build a second CSuite report for the one-to-many data. |

Still stuck? See [Getting help](../README.md#getting-help) on the main page.

> [!WARNING]
> Remember: this workbook now contains your API keys inside its query. Anyone you share the file with can refresh it — and can open the query to read your SIGNER/SIGNATURE. Share thoughtfully, or paste values instead of sharing the live workbook. See [Keep your keys safe](getting-your-api-credentials.md#keep-your-keys-safe).
