# Step 2 (Power BI): Connect Power BI to Your CSuite Report

This guide walks you through pulling a CSuite custom report into **Power BI Desktop** using [PowerBITemplate.txt](../PowerBITemplate.txt), then publishing it so it can refresh automatically in the Power BI Service.

**Before you start**, you need:

- Your four values from CSuite — if you don't have them yet, do **[Step 1: Get your API credentials](getting-your-api-credentials.md)** first.
- **Power BI Desktop** installed ([free download from Microsoft](https://www.microsoft.com/en-us/download/details.aspx?id=58494)).

Setup takes about 10 minutes.

## Part 1: Create the query

1. Open **Power BI Desktop** with a new (or existing) report file.
2. On the **Home** ribbon, click **Get data** → **Blank query**. The Power Query Editor window opens with an empty query.

   <!-- TODO: screenshot of Get data > Blank query -->

3. In the Power Query Editor, click **Advanced Editor** on the Home ribbon. A window opens showing a small amount of placeholder code.
4. Open [PowerBITemplate.txt](../PowerBITemplate.txt) (click the link, then use the copy button on GitHub), select **all** of its text, and paste it into the Advanced Editor — **replacing everything** that was there.

   <!-- TODO: screenshot of the Advanced Editor with the template pasted in -->

## Part 2: Fill in your four values

In the pasted code, find and replace these four placeholders with your values from Step 1. Replace the *entire* placeholder **including the angle brackets and asterisks**, but keep the surrounding quotation marks:

| Placeholder | Replace with | Example result |
|-------------|--------------|----------------|
| `<**ORG_PREFIX_HERE**>` | Your organization's URL prefix | `"https://yourorg.fcsuite.com"` |
| `<**SIGNER HERE**>` | Your SIGNER key | `#"SIGNER" = "abcd...wxyz"` |
| `<**SIGNATURE**>` | The report's SIGNATURE key | `#"SIGNATURE" = "AAAA...zzz="` |
| `<**REPORT ID**>` | The report_id number | `"report_id=1234"` |

5. Click **Done**. If the Advanced Editor shows a syntax error at the bottom, double-check that all quotation marks are still in place around your values.
6. If Power BI asks how to connect, choose **Anonymous** access and click **Connect**. (This is correct — your real credentials are the SIGNER/SIGNATURE inside the query itself.) If asked about **privacy levels**, choose **Organizational**.
7. Your report data should now appear as a table in the Power Query Editor. 🎉
8. Give the query a meaningful name (right-hand **Query Settings** pane → **Name**) — use the CSuite report name so future-you knows where the data comes from.
9. Click **Close & Apply** on the Home ribbon to load the data into your Power BI model.

To connect **additional CSuite reports**, repeat these steps with each report's own `report_id` and SIGNATURE (your org prefix and SIGNER stay the same).

To get fresh data at any time, click **Refresh** on the Home ribbon.

## Part 3: Publish and schedule automatic refresh

This template is written to be **"refresh-safe"**: it splits the web address into a static base URL plus a relative path, which is what the Power BI Service requires to run scheduled refreshes. (If you ever restructure the query, keep that pattern — combining the URL into one string causes the dreaded *"dynamic data source"* refresh error online.)

1. In Power BI Desktop, click **Publish** (Home ribbon) and choose your workspace.
2. In the Power BI Service (app.powerbi.com), open the workspace, find your **semantic model** (dataset), and open **Settings**.
3. Under **Data source credentials**, click **Edit credentials**, choose **Anonymous**, and check **Skip test connection** if the test fails (the source only answers correctly to the full query, so the simple test ping can fail even though refresh works).
4. Under **Refresh**, turn on **scheduled refresh** and pick your times.

No gateway is needed — CSuite is a cloud source.

## Troubleshooting

| Symptom | Likely cause and fix |
|---------|---------------------|
| **403 / Forbidden or credentials error** | A SIGNER, SIGNATURE, or report_id value was mistyped, or you copied the **Power BI Connector** Key ID/Token instead of the **Raw API** SIGNER/SIGNATURE. Re-copy from CSuite (Reports page → **Show Key**). |
| **Error mentioning a rejected timestamp** | Your computer's clock is too far off — the API rejects requests with bad timestamps. Correct your PC's clock and try again. |
| **"Dynamic data source" error when refreshing online** | The query's URL was combined into a single string. Re-paste the unmodified template, which keeps the base URL and relative path separate. |
| **Expression syntax error after pasting** | A quotation mark or comma was accidentally deleted while replacing placeholders. Re-paste the template and try again carefully. |
| **Columns or data changed unexpectedly** | Someone edited the custom report in CSuite — filters, columns, and sorting there flow straight through to Power BI. See the [best practice](getting-your-api-credentials.md#before-you-start) of flagging API reports with a special category. |
| **Data loads but a column shows values mashed together with commas** | That's a CSuite "Array" (list) field — the template converts lists to comma-separated text so the query doesn't fail. If you need those items as separate rows, ask about splitting the column, or build a second CSuite report for the one-to-many data and relate the two tables in Power BI. |

Still stuck? See [Getting help](../README.md#getting-help) on the main page.
