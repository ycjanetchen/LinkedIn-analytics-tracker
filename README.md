# LinkedIn Analytics Tracker

An automated LinkedIn post analytics system built with **Claude Cowork** and **Claude in Chrome**. It pulls your real post metrics daily, generates weekly and monthly insight reports, and displays everything in an interactive HTML dashboard — no API keys or third-party tools required.

---

## Demo

![LinkedIn Analytics Tracker demo](demo.gif)

---

## What it does

| Report | Frequency | Output file |
|--------|-----------|-------------|
| Daily post snapshot | Every day at 8 AM | `linkedin_report_log.md` |
| Weekly post summary + engagement rate | Every Monday 8 AM | `weekly_summary.md`, `engagement_rate_tracker.md` |
| Content type breakdown + best time to post | 1st of each month 8 AM | `content_type_breakdown.md`, `best_time_to_post.md` |
| Interactive dashboard | Opens anytime | `linkedin_dashboard.html` |

The dashboard lets you click between posts, switch views (Overview, Breakdown, Trend, Best Time, Top Titles), and see all metrics update instantly.

---

## Prerequisites

1. **Claude desktop app** with [Cowork mode](https://claude.ai) enabled
2. **Claude in Chrome extension** — [install here](https://chromewebstore.google.com/detail/claude-in-chrome/TODO) and sign in
3. Chrome open and logged into LinkedIn when scheduled tasks run

---

## Setup

### 1. Clone this repo

```bash
git clone https://github.com/YOUR_USERNAME/linkedin-analytics.git
cd linkedin-analytics
```

### 2. Open the folder in Claude Cowork

In the Claude desktop app, click **"Select folder"** and choose the `linkedin-analytics` folder you just cloned.

### 3. Create the three scheduled tasks

Open Claude Cowork and paste each prompt below into the chat. Claude will create the task and ask you to approve the schedule.

---

## Scheduled task prompts

### Task 1 — Daily snapshot (every day at 8 AM)

Paste this into Claude Cowork:

> **"Schedule this to run every day at 8 AM:"**
>
> ```
> You are running a daily LinkedIn post analytics report.
>
> Objective: Check the most recent LinkedIn post's metrics and append a snapshot to the daily log.
>
> Steps:
> 1. Use Claude in Chrome to navigate to https://www.linkedin.com/in/YOUR_LINKEDIN_USERNAME/recent-activity/all/
>    - If not logged in, stop and notify the user.
>    - Find the most recent original post and read its visible metrics (impressions, reactions, comments, reposts, profile clicks).
>
> 2. Also navigate to https://www.linkedin.com/analytics/creator/content/?metricType=IMPRESSIONS&timeRange=past7Days
>    - Collect the 7-day aggregate: total impressions and % change vs prior period.
>
> 3. Append a new row to linkedin_report_log.md:
>    | [today's date] | [post snippet, max 60 chars] | [publish date] | [impressions] | [reactions] | [comments] | [reposts] | [profile clicks] |
>
>    If the file doesn't exist, create it with this header first:
>    | Date | Post Snippet | Published | Impressions | Reactions | Comments | Reposts | Profile Clicks |
>    |------|-------------|-----------|-------------|-----------|----------|---------|----------------|
>
> 4. Post a brief chat summary: date, post snippet, key metrics, and one observation (e.g. engagement rate and whether it's up/down).
>
> Constraints:
> - Read only — do not post, like, or interact with anything.
> - If LinkedIn is inaccessible, write a note in the log and notify the user.
> - Append only — never overwrite existing rows.
> ```

---

### Task 2 — Weekly report (every Monday at 8 AM)

Paste this into Claude Cowork:

> **"Schedule this to run every Monday at 8 AM:"**
>
> ```
> You are running a weekly LinkedIn analytics report.
>
> Objective: Pull post metrics from LinkedIn and update two weekly report files.
>
> Steps:
> 1. Use Claude in Chrome to navigate to https://www.linkedin.com/in/YOUR_LINKEDIN_USERNAME/recent-activity/all/
>    - If not logged in, stop and notify the user.
>    - Read all visible original posts and their impression/engagement numbers from the past 7 days.
>
> 2. Also navigate to https://www.linkedin.com/analytics/creator/content/?metricType=IMPRESSIONS&timeRange=past7Days
>    - Collect: total impressions, reactions, comments, reposts, saves for the week.
>
> 3. Update weekly_summary.md:
>    - Append a new "Week of [date range]" section with a table of all posts from the past 7 days (snippet, publish date, impressions, reactions, comments, reposts, ER%).
>    - Add a week-over-week comparison row if prior week data exists in the file.
>    - ER% = (reactions + comments + reposts) / impressions × 100, rounded to 1 decimal.
>
> 4. Update engagement_rate_tracker.md:
>    - Append a new row to the weekly ER% table: week dates, impressions, reactions, comments, reposts, total engagements, ER%.
>
> 5. Post a brief chat summary: week date range, total impressions, ER%, and one observation (up/down vs prior week).
>
> Constraints:
> - Read only — do not post, like, or interact with anything.
> - If LinkedIn is inaccessible, write a note in both files and notify the user.
> - Append only — never overwrite existing rows.
> ```

---

### Task 3 — Monthly insights (1st of each month at 8 AM)

Paste this into Claude Cowork:

> **"Schedule this to run on the 1st of every month at 8 AM:"**
>
> ```
> You are running a monthly LinkedIn analytics report.
>
> Objective: Analyze accumulated post data to update two monthly insight files.
>
> Steps:
> 1. Read weekly_summary.md to extract all posts logged so far (snippet, date, type, impressions, reactions, comments, reposts).
>
> 2. Also use Claude in Chrome to navigate to https://www.linkedin.com/in/YOUR_LINKEDIN_USERNAME/recent-activity/all/
>    - Collect any posts not yet in the log. Note content type: text, image, video, or article/link.
>    - Note publish day and time if visible.
>
> 3. Update content_type_breakdown.md:
>    - Group posts by content type (text, image, video, article/link).
>    - Calculate average impressions, reactions, comments, reposts, and ER% per type.
>    - Append a new monthly snapshot table with these averages.
>    - Add a 1-line recommendation on which format is performing best.
>
> 4. Update best_time_to_post.md:
>    - Log each post's publish day and time (if available) alongside its impressions and ER%.
>    - Update the "Performance by day" and "Performance by time slot" summary tables.
>    - Add a recommendation for the best posting window based on accumulated data.
>
> 5. Post a brief chat summary: month, top content type, best posting day, and one key insight.
>
> Constraints:
> - Read only — do not post, like, or interact.
> - If a post's publish time is not visible, log the date only and note "time unavailable."
> - Append only — never overwrite existing rows.
> ```

---

## Customization

**Change your LinkedIn username:**
Replace `YOUR_LINKEDIN_USERNAME` in all three prompts with your actual LinkedIn profile slug (the part after `linkedin.com/in/`).

**Change the schedule time:**
When pasting the prompt, specify a different time — e.g. "every day at 7 AM" or "every Monday at 9 AM".

**Add a new post to the dashboard:**
Open `linkedin_dashboard.html` and find the `posts` array near the top of the `<script>` block. Copy the template comment and fill in your new post's data.

**Track a different metric as primary:**
In the daily task prompt, change `metricType=IMPRESSIONS` in the analytics URL to `metricType=ENGAGEMENT_RATE` or `metricType=REACTIONS`.

---

## File structure

```
linkedin-analytics/
├── README.md                    ← this file
├── linkedin_dashboard.html      ← interactive dashboard (open in browser)
├── linkedin_report_log.md       ← daily snapshot log (auto-appended)
├── weekly_summary.md            ← weekly post comparison (auto-appended)
├── engagement_rate_tracker.md   ← ER% trend over time (auto-appended)
├── content_type_breakdown.md    ← performance by post format (monthly)
└── best_time_to_post.md         ← best day/time analysis (monthly)
```

---

## Tips

- **Run each task once manually first** (via the Scheduled panel in Claude Cowork) to pre-approve browser permissions — this prevents future runs from pausing on permission prompts.
- Keep Chrome open and signed into LinkedIn for scheduled runs to work.
- The dashboard's post selector updates automatically as you add new posts to the `posts` array — no other changes needed.
- After 2–3 months of data, the Best Time and Content Type views become genuinely predictive.

---

*Built with [Claude Cowork](https://claude.ai) · Data source: LinkedIn Post Analytics*
