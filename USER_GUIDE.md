# Ranked Bar Card — User Documentation

## Contents

- [Quick start](#quick-start)
- [Glossary](#glossary)
- [What it does](#what-it-does)
- [Fields](#fields)
  - [Axis](#axis)
  - [Legend](#legend)
  - [Value](#value)
  - [Data label](#data-label)
  - [Percent](#percent)
  - [Detail](#detail)
  - [Sort by](#sort-by)
  - [Target](#target)
  - [Compare to](#compare-to)
  - [Tooltips](#tooltips)
  - [Footer 1 and Footer 2](#footer-1-and-footer-2)
- [Sample data model](#sample-data-model)
- [Conditional formatting with DAX (Advanced)](#conditional-formatting-with-dax-advanced)
  - [How it works](#how-it-works)
  - [Bar colour](#bar-colour)
  - [Text colours](#text-colours)
- [Settings](#settings)
  - [Card](#card)
  - [Title](#title)
  - [Legend (format card)](#legend-format-card)
  - [Category](#category)
  - [Levels](#levels)
  - [Bars](#bars)
  - [Value label](#value-label)
  - [Percent label](#percent-label)
  - [Detail (format card)](#detail-format-card)
  - [Target marker](#target-marker)
  - [Compare to (format card)](#compare-to-format-card)
  - [Footer](#footer)
  - [Text tokens](#text-tokens)
- [Interaction](#interaction)
- [Common gotchas](#common-gotchas)
- [Tips](#tips)
- [Support](#support)

---

## Quick start

Three steps to a working visual. Everything else is optional and explained below.

1. Drag your **category column** (e.g. StoreName, Country, Channel) into the `Axis` field.
2. Drag your **measure** (e.g. Total Sales Amount) into the `Value` field.
3. That's it: one bar per category, sorted largest first, with the value, its % of total and a footer.

> Want arrows like ▲ 8.2 %? Add a measure to [Detail](#detail). Want to see who moved up since last year? Add last year's measure to [Compare to](#compare-to). Want to split each bar? Add a [Legend](#legend). No helper tables or ranking DAX are needed: rank, Top N and % of total are built in.

---

## Glossary

| Term | Meaning |
|---|---|
| **Bar** | One row of the card: label, values and a bar for one Axis value |
| **Track** | The faint full-width lane behind each bar |
| **Segment** | One Legend member's part of a stacked bar |
| **Other** | The bar that adds up everything beyond Top N, or the series that groups the Legend members beyond Top N members |
| **Detail chip** | The small value next to the main value, e.g. ▲ 8.2 %. It can show any measure |
| **Target tick** | A thin mark on the track at the Target value |
| **Rank movement** | ▲2 / ▼1 / = next to a label: places gained or lost against the ranking by Compare to |
| **Compare outline** | A dashed outline on a bar at its Compare to value |
| **Average line** | A dashed line down the card at the average of the bars shown |
| **Grouped display** | With an expanded Axis hierarchy: each parent (e.g. a channel) has its own bar, with its children (its stores) indented below |
| **Token** | A placeholder such as `{total}` in the title or footer, replaced by a live value |
| **fx** | Power BI's conditional formatting button: colours set by rules, a gradient or a field value |

---

## What it does

- Ranks one bar per category and sorts it for you: by value, by name (dates and numbers sort naturally) or by any measure
- Shows the value, its % of the grand total and an optional Detail chip with 37 indicator styles (▲▼, arrows, animated trend icons)
- Top N with an "Other" bar and a "Show all" link, 11 rank badge styles (numbers, medals, trophies, crowns…), target ticks, an average line, negative values mirrored from a zero line
- Compare to any measure (last year, budget, forecast): rank movement ▲▼, an outline at the compared value and the % change
- Bars grow in on load and slide to their new rank when the data changes
- Drill down through an Axis hierarchy (e.g. Channel › Store) with Power BI's drill buttons, shown grouped like a matrix (a bar for every level) or flat
- Stacked or 100 % stacked bars when a Legend field is bound
- Cross-filters other visuals, is filtered and highlighted by them, supports drillthrough, tooltips, keyboard and high contrast

---

## Fields

Bind these in the Visualizations pane. Required fields are marked ✅; everything else is optional.

### Axis

- **What it is**: the category to rank. One bar per unique value
- **Required**: ✅
- **How to use**: drag a column, e.g. `DimStore[StoreName]`. Field parameters work too: the default title (`{value} by {axis}`) follows the field the parameter selects
- **Drill down**: add several fields (e.g. Channel, then StoreName) and use the drill buttons in the visual header. After **Expand all down one level**, [Category](#category) → Hierarchy display decides the layout:
  - **Grouped** (default): like a matrix in compact layout. Each channel has its own bar, value, % and Detail, and its stores are listed below it, indented, each ranked within its channel. [Levels](#levels) shows or hides each level's bar and labels
  - **Flat**: only the stores, ranked together and labelled with their path, e.g. `Online › Contoso Asia Online Store`

### Legend

- **What it is**: splits each bar into stacked segments, one per member
- **Required**: ⚪
- **How to use**: drag a column with few values, e.g. `DimStore[Channel]`
- **Special treatment**: the largest members by grand total keep their own colour, and the rest are grouped as "Other". How many is up to you: [Legend (format card)](#legend-format-card) → Top N members (default 8, 0 shows every member) and "Other" series. Each drawn member gets a colour picker, up to 50. Negative values can't be stacked: a bar with a negative member is drawn as one bar for its total, in the Negative colour when the total is below zero

### Value

- **What it is**: the number behind bar length, ranking and % of total
- **Required**: ✅
- **How to use**: bind a numeric measure. Its format string (`$#,0`, `0.0%`, …) is used everywhere
- **Special treatment**: % of total uses Power BI's own grand total, so it stays exact for averages, distinct counts and ratios too
- **DAX example**:
  ```dax
  Total Sales Amount = SUM(FactSales[SalesAmount])
  ```

### Data label

- **What it is**: text shown as the value label instead of Value. Bars still scale on Value
- **Required**: ⚪
- **How to use**: bind a measure, e.g. units while bars scale on revenue
- **DAX example**:
  ```dax
  Units Sold = SUM(FactSales[Quantity])
  ```

### Percent

- **What it is**: replaces the calculated share with your own percentage
- **Required**: ⚪
- **How to use**: bind a measure formatted as a percentage, e.g. share of the store's country
- **DAX example**:
  ```dax
  Share of Country = DIVIDE([Total Sales Amount], CALCULATE([Total Sales Amount], ALLEXCEPT(DimStore, DimStore[Country])))
  ```

### Detail

- **What it is**: the Detail chip next to the value. Any measure works: growth %, orders, margin, rank change
- **Required**: ⚪
- **How to use**: bind a measure; it is shown in its own format string
- **Special treatment**: the sign picks the indicator (▲ / ▼ / neutral) and, if [Colour by sign](#detail-format-card) is on, the good / bad colour. Turn the indicator to None for plain numbers
- **DAX example**:
  ```dax
  Growth vs PY = DIVIDE([Total Sales Amount] - [Sales PY], [Sales PY])
  ```

### Sort by

- **What it is**: ranks the bars (rank numbers and Top N) by this measure instead of Value
- **Required**: ⚪
- **How to use**: bind a measure, then set [Category](#category) → Sort by → Sort by field to also order the bars by it
- **DAX example**:
  ```dax
  Orders = DISTINCTCOUNT(FactSales[OrderID])
  ```

### Target

- **What it is**: draws a target tick on each track; the tooltip shows the gap, e.g. "+5.3 % vs target"
- **Required**: ⚪
- **How to use**: bind a measure on the same scale as Value
- **Special treatment**: hidden on 100 % stacked bars, where a target has no meaning
- **DAX example**:
  ```dax
  Sales Target = [Sales PY] * 1.05
  ```

### Compare to

- **What it is**: any measure to compare each bar with: last year, budget, forecast, another scenario
- **Required**: ⚪
- **How to use**: bind a measure on the same scale as Value. Each bar then shows:
  - **Rank movement** next to its label: ▲2 when it ranks 2 places higher than it does by Compare to, ▼1 when lower, = when unchanged
  - **An outline** at its Compare to value, so you can see how far it moved
  - **The % change** in the Detail chip, when the Detail field is empty
- **Special treatment**: the tooltip lists the Compare to value, the change and both ranks (e.g. `#2 → #4`). With a Legend, the outline sits around the whole stack. Hidden on 100 % stacked bars
- **DAX example**:
  ```dax
  Sales PY = CALCULATE([Total Sales Amount], SAMEPERIODLASTYEAR(DimDate[Date]))
  ```

### Tooltips

- **What it is**: extra measures listed in the tooltip, each in its own format
- **Required**: ⚪
- **How to use**: drag up to 10 measures. Report-page tooltips work too: Format → General → Tooltips → Type → Report page

### Footer 1 and Footer 2

- **What it is**: totals you can place in the footer or title with the `{m1}` and `{m2}` tokens
- **Required**: ⚪
- **How to use**: bind a measure, then type e.g. `{n} stores · {m1} orders` in [Footer](#footer) → Left
- **Special treatment**: the total comes from Power BI's grand total, so it is correct for any measure

---

## Sample data model

Every DAX example in this guide uses this star schema:

```
   DimStore                                  DimDate
  +------------+                            +------+
  | StoreKey   | 1                        1 | Date |
  | StoreName  |----+                  +----| Year |
  | Country    |    |                  |    +------+
  | Channel    |    *                  *
  +------------+  +-------------------------------+
                  |           FactSales           |
                  | StoreKey | Date | OrderID     |
                  | SalesAmount | Quantity        |
                  +-------------------------------+
```

**Measures**

```dax
Total Sales Amount = SUM(FactSales[SalesAmount])
Sales PY           = CALCULATE([Total Sales Amount], SAMEPERIODLASTYEAR(DimDate[Date]))
Growth vs PY       = DIVIDE([Total Sales Amount] - [Sales PY], [Sales PY])
Sales Target       = [Sales PY] * 1.05
Orders             = DISTINCTCOUNT(FactSales[OrderID])
Units Sold         = SUM(FactSales[Quantity])
```

Mental model: with `StoreName` on Axis, every measure is evaluated once per store, plus once for the grand total that drives % of total and the footer totals.

---

## Conditional formatting with DAX (Advanced)

> **Advanced — skip unless you want data-driven colours.** [Bars](#bars) → Colour mode already covers single colour, theme palette and good / bad by Detail sign. Read on only if you need a rule or a DAX measure to pick colours.

### How it works

- **Where**: the **fx** button next to Bars → Bar colour, Category → Colour, Value label → Colour, Percent label → Colour and Detail → Colour (no sign)
- **Per bar**: Power BI evaluates your rule once per Axis value, so every bar can get its own colour
- **Field value**: the measure must return a hex string starting with `#`, e.g. `"#10B981"`
- **Rules / gradient**: pick a measure from the model and set ranges or a colour scale in Power BI's dialog

### Bar colour

- **When it applies**: Colour mode = Single colour. Bars below zero keep the Negative colour, and with a Legend field the colours come from the Legend card instead
- **DAX example** (field value):
  ```dax
  Bar Colour = IF([Total Sales Amount] >= [Sales Target], "#10B981", "#F59E0B")
  ```
- **Tip**: for green / red by growth, Colour mode → Detail sign gives the same result without DAX

### Text colours

- **What they do**: colour the category label, value label or percent label of each bar separately
- **Detail colour**: the fx on Detail → Colour (no sign) applies only when Colour by sign is off
- **DAX example** (field value, flags stores below target in red):
  ```dax
  Label Colour = IF([Total Sales Amount] < [Sales Target], "#EF4444", "#D3DAE3")
  ```

---

## Settings

> **Five settings you'll touch most**
>
> 1. **Category → Top N** and **"Other" bar**: show the top 10 and group the rest.
> 2. **Bars → Colour mode**: Single colour, Theme palette or Detail sign (good / bad).
> 3. **Detail (format card) → Sign indicator**: pick from 37 indicator styles.
> 4. **Value label → Decimals**: e.g. 1 for `$36.2M`.
> 5. **Card → Theme**: Dark, Light or Transparent.

The format pane has 10 cards, in this order, plus **Levels** in Grouped display and **Compare to** when that field is bound. A greyed-out setting can't apply to the current fields: hover it to see why.

### Card

- **Theme** — Dark (default), Light, or Transparent (no background or border, for dark report backgrounds)
- **Font** — font family for the whole card. Default Segoe UI
- **Border** — show the card border. Default on
- **Corner radius** — card corners in pixels. Default 14
- **Padding** — inner spacing in pixels. Default 18
- **Animate changes** — bars grow in on load and slide to their new rank when the data changes. Default on. Skipped when Windows' reduce-motion setting is on

### Title

- **Title** — default `{value} by {axis}`. Accepts [tokens](#text-tokens); leave empty for no title
- **Title size** — default 15
- **Subtitle** — empty by default. Accepts tokens
- **Subtitle size** — default 12

### Legend (format card)

- **Show** — show the legend in the card. Default on
- **Position** — Header right, Below title or Above footer
- **Positive label / Negative label** — legend text for Detail-sign colours. Default Growing / Declining. Greyed out while a Legend field is bound
- **Stack mode** — Stacked or 100 % stacked (Legend field bound)
- **Segment gap** — pixels between segments. Default 1
- **Top N members** — how many members keep their own colour, ranked by grand total. Default 8; **0 shows every member**
- **Top N scope** — **Across all bars** (default: the members with the biggest grand totals, so every bar shows the same ones) or **Per bar** (each bar draws its own biggest members; the legend then lists every member some bar draws). Greyed out when Top N members is 0
- **"Other" series** — groups the members beyond Top N into one segment. Default on. Turn it off to draw only the top members: each bar then shows the rest of its total as empty track, so the segments stay truthful
- **Member colours** — one picker per Legend member, seeded from the report theme

### Category

- **Label position** — Above bar (default) or Inline left
- **Font size** — default 13. **Colour** — has fx
- **Label width (inline)** — label column width in Inline left. Default 120
- **Hierarchy display** — with an expanded Axis hierarchy: Grouped (default, a bar per level with children indented under their parent) or Flat (lowest level only, labelled with its path). Greyed out until a second Axis field is expanded
- **Indent per level** — how far each level is indented in Grouped display. Default 16
- **Rank style** — badge before each label: Off (default), Number, Pill (#1), Circled (①), Solid (❶), Medals, Trophy (#1 only), Crowns, Stars, Ribbons or Flames. Icon styles decorate the top 3 and show the number after that. The rank column widens for 100+ bars so labels stay aligned
- **Sort by** — Value, Axis (A→Z, dates and numbers in natural order) or Sort by field. **Direction** — Descending or Ascending
- **Top N** — show only the N largest (ranked by Sort by field, else Value). 0 shows all
- **Top N scope** — Grouped display only: **Per group** (default: the top N inside every group, e.g. 5 stores per channel) or **Across all groups** (the top N bars of the whole card; a group whose bars are all cut disappears). Greyed out in Flat display, where there is only one scope, and when Top N is 0
- **"Other" bar** — add up the rest into one bar. **"Other" label** — default Other
- **"Show all" link** — with Top N, a link under the bars shows every bar and back to the top N. Default on. It resets when Top N changes

### Levels

Shown in Grouped display only: one group per Axis level on screen, named after its field (e.g. **Level 1 · Channel**, **Level 2 · StoreName**). Each has:

- **Bar**, **Category label**, **Value label**, **Percent label**, **Detail** — show or hide that part on this level's rows. All on by default. Value, Percent and Detail also need their own card switched on

Up to 5 levels have their own settings; deeper levels follow Level 5.

How Grouped display works:

- A parent's bar, value and % come from Power BI's own total for that group, so they're right for any measure (averages, ratios, distinct counts), not a sum of the rows below
- Each level is scaled to its own largest bar, so stores stay readable next to channel totals
- Ranking, sorting, rank badges, Compare to movement and Top N work within each group: Top 5 shows the 5 best stores of every channel, and the rest fold into that channel's "Other"
- Clicking a parent filters by that parent; clicking a child filters by the child within its parent
- Spacing: [Bars](#bars) → Row gap sets the space inside a group, Group gap the space between groups

### Bars

- **Height** — default 10. **Corner radius** — default 3
- **Row spacing** — Fit to height (spreads the rows, scrolls when they don't fit) or Fixed + scroll
- **Row gap** — space between rows with Fixed + scroll, or with Fit to height once the rows don't fit and the card scrolls. In Grouped display it is the space between the rows **inside** a group. Default 10. Greyed out while Fit to height is spreading the rows
- **Group gap** — Grouped display only: extra space **between** groups, on top of the row spacing. Default 12. With three levels it separates the top groups and the groups inside them, but never pushes a first child away from its parent
- **Colour mode** — Single colour, Theme palette or Detail sign. Greyed out while a Legend field is bound (members have their own colours)
- **Bar colour** — has fx. **Negative colour** — for bars below zero
- **Scale** — Largest bar (default) or Total (100 % track, bars show their share of the whole)
- **Track** / **Track colour** — the lane behind each bar
- **Min visible width** — smallest bar in pixels so tiny values stay visible. Default 2
- **Gradient** — bars fade in from where they start. Greyed out while a Legend field is bound
- **Glow** — a soft halo in each bar's colour, segments included
- **Average line** / **Average line colour** — a dashed line at the average of the bars shown ("Other" excluded), labelled e.g. `Avg $11.2M`. Greyed out on 100 % stacked bars and in Grouped display (each level has its own scale)

### Value label

- **Show** — default on
- **Display units** — Auto, None, Thousands, Millions, Billions, Trillions
- **Decimals** — Auto (from the format string) or 0–4
- **Font size** — default 14. **Colour** — has fx

### Percent label

- **Show** — default on
- **Base** — All rows (default: share of the grand total, filters respected) or Visible rows (shares of the bars shown add to 100 %)
- **Decimals** — default 1. **Font size** — default 11. **Colour** — has fx

### Detail (format card)

- **Show** — default on. **Style** — Text or Tinted pill
- **Sign indicator** — 37 styles: triangles, arrows, chevrons, plus / minus, check / cross, animated trend icons, None
- **Colour by sign** — good / bad / neutral colours. **Higher is better** — turn off when lower values are good (e.g. costs)
- **Neutral band ±** — values within this amount count as neutral, in the measure's own units (0.005 = ±0.5 % for a percentage)
- **Decimals**, **Font size**, **Good / Bad / Neutral colours**, **Colour (no sign)** (has fx)

### Target marker

- **Show** — default on (only drawn when a Target field is bound). Greyed out on 100 % stacked bars
- **Colour** — default follows the theme
- **Width** — tick width in pixels. Default 2

### Compare to (format card)

Shown only when a [Compare to](#compare-to) field is bound.

- **Outline at its value** — an outline at each bar's Compare to value. Default on. Greyed out on 100 % stacked bars
- **Outline colour** / **Outline width** (0.5–6 px, default 1.5) / **Outline style** (Dashed, Dotted, Solid) — how the outline is drawn. Greyed out while the outline is off
- **Rank movement** — ▲ / ▼ / = next to each label. Default on
- **Fill Detail when empty** — the Detail chip shows the % change from Compare to. Default on. Greyed out while a Detail field is bound (your Detail measure wins)

### Footer

- **Left** — default `{axis}: {count}`. **Right** — default `Total {total}`. Both accept [tokens](#text-tokens)
- **Right in accent colour** — for a call to action such as "View details →"
- **Divider line** — line above the footer. Default on
- **Font size** — default 11

### Text tokens

| Token | Replaced by |
|---|---|
| `{axis}` / `{value}` / `{legend}` / `{compare}` | Field names as shown in the field wells |
| `{n}` / `{count}` | Bars shown (without "Other") / all Axis values |
| `{total}` | Grand total of Value |
| `{m1}` / `{m2}` | Grand total of Footer 1 / Footer 2 |
| `{top:2}` / `{topShare:2}` | Names of the top 2 bars / their combined share (any number works) |

---

## Interaction

- **Click a bar** — filters or highlights other visuals by that Axis value. Ctrl+click adds bars; click the same bar or empty space to clear
- **Click a segment** — filters by Axis value **and** Legend member
- **Click a legend member** — filters by that member across all bars
- **Right-click** — Power BI's menu: Drill through, Include, Exclude, Copy. A bar passes its Axis value; a segment passes Axis + Legend
- **Show all / Show top N** — the link under the bars (with Top N) expands and collapses the list
- **Drill down** — with several Axis fields, use the drill buttons in the visual header, or turn on drill mode and click a bar. In Grouped display, clicking a parent filters by that parent
- **Other visuals** — filter this card, or highlight it (bars fade and the matching part stays lit), per Format → Edit interactions
- **Keyboard** — Tab to the bars (one tab stop), ↑ ↓ between bars, Enter or Space to select, Esc to clear, Shift+F10 for the menu. Legend members are separate tab stops: Enter or Space selects one

---

## Common gotchas

- **"Percentages don't add up to 100 %"** → Percent label → Base is All rows, which includes bars hidden by Top N. Turn on the "Other" bar or switch Base to Visible rows.
- **"Some Legend members are grouped as Other"** → Legend → Top N members decides how many keep their own colour (default 8). Raise it, or set 0 to show every member. Past a dozen or so, segment colours get hard to tell apart, and only the first 50 members get a colour picker.
- **"A scrollbar appeared"** → Fit to height switches to scrolling when the rows can't fit. Use Top N, a smaller bar height, or a taller visual.
- **"Showing the first 10,000 values"** → the Axis has more values than the visual loads. Use Top N or a filter.
- **"Too small"** → the card needs at least 200 × 110 pixels.
- **"A setting is greyed out"** → hover it for the reason. With a Legend field, Colour mode, Bar colour, Gradient and Positive / Negative labels don't apply (set colours in [Legend (format card)](#legend-format-card)). On 100 % stacked bars, Average line, Target and the Compare to outline don't apply because every bar is full width.
- **"A parent's bar looks short next to its children"** → in Grouped display each level has its own scale: compare bars within a level, not across levels.
- **"A parent shows no bar or value"** → Power BI sent no total for that group. Check the report hasn't turned subtotals off for the visual.
- **"Rank movement shows = everywhere"** → the Compare to measure ranks the bars in the same order as Value. Check it returns the comparison period (e.g. last year), not the current one.

> Looking for fx / DAX colour questions? They're inline in [Conditional formatting with DAX](#conditional-formatting-with-dax-advanced).

---

## Tips

- **Format strings** — `$#,0.00`, `0.0%`, `MMMM yyyy` are honoured in labels, footer and tooltips; Display units and Decimals override them
- **Field parameters** — bind one to Axis and the default title follows the selected field
- **Design card look** — Value label → Decimals 1, Bars → Colour mode Theme palette, Footer → `{n} channels · {m1} orders`
- **Top countries look** — Top N 6, Detail = Growth vs PY, Colour mode Detail sign, Percent label off
- **Growth without DAX** — put `Sales PY` in Compare to and leave Detail empty: the Detail chip shows the % change
- **Leaderboard look** — Rank style Medals, Compare to = last year, Bars → Glow
- **High contrast** — Windows high-contrast themes are applied automatically; Legend members alternate between filled and outlined so neighbours stay distinguishable
- **Bookmarks** — keep and restore the selection

---

## Support

- **Email**: [support@danbistudio.com](mailto:support@danbistudio.com)
- **Website**: [danbistudio.com](https://danbistudio.com)
- **Response time**: within 2 business days

> Ranked Bar Card is free: every feature is included.
