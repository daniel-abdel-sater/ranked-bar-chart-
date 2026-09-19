# Ranked Bar Card — Privacy Policy

**Publisher:** Daniel Abdel Sater (DanBi Studio)
**Visual:** Ranked Bar Card (Power BI custom visual)
**Effective date:** 2026-09-19
**Applies to:** the Ranked Bar Card custom visual, distributed as a direct .pbiviz download (and via Microsoft AppSource once it is published there).

DanBi Studio respects your privacy. This policy describes how the Ranked Bar Card Power BI custom visual handles data when it is loaded into a report. It applies only to the visual itself — Power BI, the host report, and any data sources connected to that report are governed by your organization's and Microsoft's own privacy terms.

## 1. How Ranked Bar Card handles your data

Ranked Bar Card runs entirely inside Power BI's sandboxed iframe. It receives only the values you bind to its field wells (Axis, Legend, Value, Data label, Percent, Detail, Sort by, Target, Compare to, Tooltips, Footer 1 and Footer 2), computes rankings, percentages and layout in your browser, and draws the result. Selections, cross-filtering, drillthrough and tooltips go through Power BI's own services and stay inside Power BI. The visual does **not**:

- send any data over the network — `privileges` in `capabilities.json` is empty, meaning the visual has no permission to make external requests
- include any analytics, telemetry, or tracking SDK
- contact any external service, API, font CDN, image host, or map tile provider
- log, store, or transmit your data to DanBi Studio or any third party

In short: the data you bind never leaves your browser through this visual.

## 2. Data Sharing

DanBi Studio does not sell, rent, or share your data with any third party.

If you install the visual from Microsoft AppSource, Microsoft may collect aggregate licensing and usage telemetry about how the visual is downloaded and instantiated. That data flow is between you (or your organization) and Microsoft, governed by [Microsoft's privacy statement](https://privacy.microsoft.com/privacystatement), and is outside the scope of this policy.

## 3. Cookies and Local Storage

Ranked Bar Card does not set cookies and does not use browser `localStorage` or `sessionStorage`.

The visual uses Power BI's `persistProperties` API in one narrow case: on first load it saves `subTotals.rowSubtotals = true` and `subTotals.columnSubtotals = true` to the report, so Power BI's matrix engine sends the grand totals used for "% of total" and footer totals. These are two on/off flags — no personal data, no user-identifying information, no session state.

Like any visual, the format-pane choices you make are saved by Power BI in the report. Colours you pick for individual Legend members are saved keyed to that member's value (for example "Online"). All of this lives inside the .pbix file itself, not on your device or our servers, and is removed when you remove the visual from the report.

## 4. Your Data Rights

Because Ranked Bar Card does not collect personal data, DanBi Studio is not a "data controller" with respect to this visual under GDPR Article 4 or an equivalent role under the CCPA. There is no personal data held by us to access, rectify, port, restrict, or erase.

The data your report contains is governed by your organization's data-controller policies and Microsoft's terms — not by us.

## 5. Retention

DanBi Studio retains zero data because Ranked Bar Card collects zero data.

## 6. Security

Ranked Bar Card runs inside Power BI's sandboxed iframe, isolated from the host page and other visuals. It has no network egress (its `privileges` array is empty), so the data you bind cannot be exfiltrated through this visual. The values saved via `persistProperties` and the format pane are stored by Power BI itself in the .pbix file and inherit Power BI's encryption-at-rest and in-transit guarantees.

## 7. Children's Privacy

Ranked Bar Card is a business-intelligence tool intended for use within Power BI by professional, educational, and organizational users. It is not directed at children under 13, and we do not knowingly collect personal data from children.

## 8. Changes to this policy

We may update this policy as the visual evolves or as legal requirements change. The "Effective date" at the top of this document reflects the most recent version. Material changes will be noted in the visual's release notes on its distribution page (the GitHub repository, and the AppSource listing once published). Continued use of the visual after a change constitutes acceptance of the updated policy.

## 9. Governing Law

This policy is governed by the laws of Lebanon, without regard to conflict-of-laws principles. Any dispute arising out of or relating to this policy or the visual will be subject to the exclusive jurisdiction of the courts located in Lebanon.

## 10. Contact

Questions, concerns, or privacy-rights requests: **support@danbistudio.com**
