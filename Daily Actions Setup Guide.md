# Daily Actions Tracker — Full Setup Guide

Everything below builds one system: a Notion database called **Daily Actions Tracker**, one row per day (June 21 – July 9), with tick boxes, reflections, a live % score, a red/green status, a squares overview, and a clickable "Daily Protocol Checklist" sub-page on every day.

Do steps 1–6 once. They're easiest on a computer (notion.so or the desktop app) — formulas and templates are fiddly on a phone keyboard. Once built, ticking things off day-to-day works fine on mobile.

---

## 1. Import the data

1. In Notion's sidebar, click **Import** (bottom-left area, or **+** next to your workspace name → Import).
2. Choose **CSV**, select `Daily Actions Tracker.csv`.
3. Notion creates a database with 19 rows, one per date, June 21 → July 9.

## 2. Fix column types

Click each column header → **Edit property** → set **Type** to:

| Column | Type |
|---|---|
| Date | Date |
| Sleep Schedule, Work Grind, Cold Calls, Car Washing, Gym, Eat Right | Checkbox |
| Tried My Best, Moved With Speed, Wasted Time, Biggest Win, Do Differently, Thanked God, Good Person | Text |
| Money Made (GBP) | Number |

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

100% only happens when all 6 actions are ticked **and** all 7 reflections have text written in them.

## 4. Add the "Day Status" formula (red/green color)

**+ Add property → Formula**, name it `Day Status`, paste:

```
if(
  prop("% Complete") = 100,
  background("✅ 100% Complete", "green"),
  background(format(round(prop("% Complete"))) + "% Complete", "red")
)
```

> If `background()` isn't in the autocomplete on your Notion version, use `color()` instead — same syntax.

## 5. Turn on the squares overview

1. Next to the "Table" tab at the top, click **+ Add view → Gallery**, name it "Overview".
2. **Customize → Properties**: turn off everything except `Date` and `Day Status`.
3. **Sort**: Date, Ascending. Card size: Small.

You now have a grid of squares, June 21 → July 9, red until a day is finished, green at 100%.

## 6. Add the Daily Protocol Checklist as a clickable sub-page

This makes the big checklist (Block 1–7, Daily Rules, Conditional, Weekly) open as its own page from a link at the top of each day, instead of cluttering the row with 30+ extra checkboxes.

**Build the template once:**

1. Open the database, click the small **˅** arrow next to the **New** button (top-right of the table) → **+ New template**.
2. Name the template `Daily Protocol Checklist`.
3. Inside the blank template page, type `/page`, press Enter, and name the new sub-page `Daily Protocol Checklist`.
4. Click into that sub-page and paste the full contents of `Daily Protocol Checklist.md`. Notion auto-converts `- [ ]` into real checkboxes, `**bold**` into bold text, and `---` into dividers — so it lands clean and tickable, same as the rest of the system.
5. Go back to the template settings and turn on **"Set as default template"**. Every new day created from now on automatically gets this sub-page already inside it — genuinely fresh each time, because each day is its own page.

**Apply it to the 19 days you already imported** (they were created before the template existed, so they're empty):

1. Open a day's row.
2. Near the top of the empty page you'll see a suggestion to **Apply template** / **+ Templates** — click it, choose `Daily Protocol Checklist`.
3. Repeat for each of the 19 days (one-time, a minute or two total).

Now opening any day shows the link at the top — click it, tick things off, go back, and it's saved automatically (Notion always autosaves).

### Optional: make the protocol count toward your %

Ticks inside that sub-page can't be read by a formula on the main row — Notion formulas only see properties, not sub-page content. If you want completing the protocol to actually move the % needle:

1. Add one more property to the main database: **+ Add property → Checkbox**, name it `Protocol Followed`.
2. Once you finish the sub-page checklist for the day, tick that one box on the main row yourself.
3. Add `if(prop("Protocol Followed"), 1, 0) +` into the `% Complete` formula (step 3) and change the divisor from `13` to `14`.

This is optional — skip it if you're happy with the protocol checklist being separate from the score.

## 7. Daily use

- **Table view**: tick the 6 action boxes, write the 7 reflections, click the protocol link at the top if you added it to that page.
- **Overview view**: glance at the squares to see the whole stretch at once.
