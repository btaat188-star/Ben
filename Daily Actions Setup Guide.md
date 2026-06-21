# Daily Actions Tracker — Setup Guide

This replaces the old single-page checklist with a **Notion database** (one row = one day, June 21 – July 9). Databases are required because plain pages can't auto-calculate a % or change color on their own — checkbox/formula properties can.

## 1. Import the data

1. In Notion: **⋯ menu → Import → CSV**
2. Choose `Daily Actions Tracker.csv`
3. Notion creates a database with 19 rows, one per day.

## 2. Fix column types (one-time)

Click each column header → **Edit property** → set type:

| Column | Type |
|---|---|
| Date | Date |
| Sleep Schedule, Work Grind, Cold Calls, Car Washing, Gym, Eat Right | Checkbox |
| Tried My Best, Moved With Speed, Wasted Time, Biggest Win, Do Differently, Thanked God, Good Person | Text |
| Money Made (GBP) | Number |

These are your tick boxes and reflection fields — tick the checkboxes and write in the text fields directly in the table, or open a day as a page and they show as properties at the top.

## 3. Add the "% Complete" formula

**+ Add property → Formula**, name it `% Complete`, paste:

```
round(
  (
    if(prop("Sleep Schedule"), 1, 0) +
    if(prop("Work Grind"), 1, 0) +
    if(prop("Cold Calls"), 1, 0) +
    if(prop("Car Washing"), 1, 0) +
    if(prop("Gym"), 1, 0) +
    if(prop("Eat Right"), 1, 0) +
    if(empty(prop("Tried My Best")), 0, 1) +
    if(empty(prop("Moved With Speed")), 0, 1) +
    if(empty(prop("Wasted Time")), 0, 1) +
    if(empty(prop("Biggest Win")), 0, 1) +
    if(empty(prop("Do Differently")), 0, 1) +
    if(empty(prop("Thanked God")), 0, 1) +
    if(empty(prop("Good Person")), 0, 1)
  ) / 13 * 100
)
```

This means a day only hits 100% when **all 6 actions are ticked AND all 7 reflection fields have something written in them** — exactly as you wanted.

## 4. Add the "Day Status" formula (the red/green color)

**+ Add property → Formula**, name it `Day Status`, paste:

```
if(
  prop("% Complete") = 100,
  background("✅ 100% Complete", "green"),
  background(format(round(prop("% Complete"))) + "% Complete", "red")
)
```

This colors the text green at 100%, red otherwise, and updates live as you tick boxes / fill in text — no manual updating needed.

> If your Notion version doesn't show `background()` in the formula autocomplete, use `color()` instead — same syntax, colors the text instead of highlighting it.

## 5. Turn on the "squares" overview

1. Top left of the database → **+ Add a view → Gallery**
2. Name it "Overview"
3. **Customize → Properties**: turn off everything except `Date` and `Day Status`
4. **Sort**: Date, Ascending
5. Card size: Small

Now you have a clean grid of squares from June 21 → July 9, each showing the date and a colored % complete badge — red until everything's ticked and filled in, green at 100%.

## 6. Daily use

Switch back to the **Table** (or **List**) view to tick off actions and write reflections for today. The Gallery view updates automatically.
