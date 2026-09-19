# Ranked Bar Card for Power BI

**A free Power BI custom visual that turns a category and a measure into a ranked, sorted bar card: value, % of total and a Detail chip on every row, with no helper measures.**

Power BI's bar chart is powerful, but making it rich, informative and clear usually takes workarounds: RANKX, "max" measures, phantom bars and a lot of formatting. Ranked Bar Card builds all of that in, so you can show a lot of insight without overwhelming the reader.

**Latest version: 1.3.1.0** · Free, every feature included

---

## Download

[**rankedBarCard95E169D0FC7F81DEB9B84590B3270B98.1.3.1.0.pbiviz**](rankedBarCard95E169D0FC7F81DEB9B84590B3270B98.1.3.1.0.pbiviz). Open the file, then click **Download raw file**.

## Install

**Power BI Desktop**
1. Open your report.
2. In the **Visualizations** pane, click **•••** (Get more visuals) → **Import a visual from a file**.
3. Pick the `.pbiviz` file you downloaded. The Ranked Bar Card icon appears in the Visualizations pane.

**Power BI Service**: open a report in edit mode, then follow the same steps in its Visualizations pane.

> If the import is blocked, your Power BI admin has turned off visuals imported from files. The setting is *Admin portal → Tenant settings → Allow visuals created using the Power BI SDK*.

## Quick start

1. Drag a **category** (e.g. StoreName, Country, Channel) into **Axis**.
2. Drag a **measure** (e.g. Total Sales Amount) into **Value**.

That's it: one bar per category, sorted largest first, with the value, its % of total and a footer. Everything else is optional.

## Features

- **Ranking built in**: bars sorted by value, by name (dates and numbers in natural order) or by any measure. No RANKX, no helper tables.
- **Rich rows**:
  - value and % of total on every bar
  - a **Detail** chip that shows any measure (growth, margin, orders…) with 37 sign-indicator styles
- **Top N + "Other"**, with a **Show all** link that expands the list in place.
- **Rank badges**: numbers, pills, circled and solid numbers, medals, a trophy, crowns, stars, ribbons or flames.
- **Compare to** any measure (last year, budget, forecast):
  - rank movement ▲2 / ▼1
  - an outline at the compared value
  - the % change, automatically
- **Animated re-ranking**: bars slide to their new rank when a filter or slicer changes.
- **Target ticks** and an **average line**.
- **Legend**: stacked or 100 % stacked segments per member.
- **Drill down** through an Axis hierarchy:
  - **Grouped**, like a matrix: each parent has its own bar, with its children indented below
  - **Flat**: the lowest level with path labels
  - per-level control of bars and labels
- **Works like a native visual**:
  - two-way cross-filtering and highlighting
  - right-click drillthrough
  - tooltips, including report-page tooltips
  - bookmarks
- **Themes**: Dark, Light or Transparent, plus Gradient and Glow bar styles.
- **Accessible**: keyboard navigation, screen-reader labels, Windows high-contrast support.
- **Scales** to 10,000 categories.

## Documentation

- [**User Guide**](USER_GUIDE.md): every field, setting and interaction, with DAX examples and common gotchas.
- [**Privacy Policy**](PRIVACY_POLICY.md): the visual sends no data anywhere, uses no telemetry and makes no external requests.

## Support

- **Email**: [support@danbistudio.com](mailto:support@danbistudio.com)
- **Website**: [danbistudio.com](https://danbistudio.com)

Made by **Daniel Abdel Sater** · [DanBI Studio](https://danbistudio.com)
