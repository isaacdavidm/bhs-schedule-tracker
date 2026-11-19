# BHS Bell

A schedule tracker for Barcelona High School, live at [bhsbell.com](https://bhsbell.com).

Open it during the school day and it tells you which period you're in, when it ends, and what's next. It knows about weekends, holidays and special schedules (half days, events), so it won't tell you to go to Period 3 on La Diada.

## Pages

| Page | File | What it does |
| --- | --- | --- |
| Home | `index.html` + `script.js` | Live view of the current and next period. Refreshes every minute. The background color shows the day type: light blue for a regular day, orange for a special schedule, beige for no school. |
| Calendar | `calendar.html` + `calendar.js` | The school-year calendar, embedded from a published Google Slides deck. It opens on the slide for the current month. |
| Countdown | `countdown.html` + `countdown.js` | School days left until the last day (June 16, 2027), with a progress bar for the year. Weekends and holidays aren't counted. |
| Lobo | `lobo.html` | The latest issue of the Lobo, the school magazine, as an embedded PDF with open and download buttons. Past issues are in `lobo/`. |
| Contact | `contact.html` | How to send feedback or report a bug. |
| About | `about.html` | About the two student developers. |
| Privacy | `privacy.html` | Privacy policy (GDPR). Every page shows a cookie consent banner (`cookie-consent.js`). |

## How it works

It's a static site: plain HTML, CSS and JavaScript, no framework and no build step. Everything the site knows about the school year is in **`scheduleData.json`**:

- `schedule.period_times`: the regular day, with start and end times (24h `HH:MM`) for periods 1–4, homeroom, lunch and passing periods.
- `holidays`: dates with no school (`YYYY-MM-DD`, a name and a type).
- `special_schedules`: dates that use a different timetable. Each one has a `schedule_override.period_times` that replaces the regular day.

`script.js` loads the JSON, checks today against weekends, `holidays` and `special_schedules` (in that order), then works out the current period from the device's local time.

## Updating for a new school year

At the start of each year:

1. In `scheduleData.json`, update `school_info.academic_year`, replace `holidays` with the new year's dates, and clear out last year's `special_schedules`.
2. If the bell times changed, edit `schedule.period_times`.
3. In `countdown.js`, update `endDate` and `schoolYearStart`. Also change the end date shown in `countdown.html`.
4. If there's a new calendar deck, update the iframe `src` in `calendar.html`. `calendar.js` assumes slide 1 is September and slide 10 is June.

To add a special schedule (e.g. a half day), add an entry to `special_schedules` with the date, a name, and the full `period_times` for that day. Period keys that are numbers show as "Period N". Other keys (`homeroom`, `lunch`, `passing_period_*`, `party`, `concert`, `market`, `debrief`) get friendly names in `updateDisplay()` in `script.js`. If you add a new kind of block, add its label there.

To publish a new Lobo issue, put the PDF in `lobo/` and update the four paths in `lobo.html`.

## Running locally

The page loads `scheduleData.json` with `fetch`, so opening `index.html` straight from Finder won't work. Run a local server from the repo root instead:

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000.

To test a particular time or date, temporarily change `const now = new Date();` in `updateDisplay()` (in `script.js`) to something like `new Date('2026-11-17T10:30')`.

## Deployment

The site is hosted on Netlify. It deploys automatically on every push to `main`, and the domain is registered with GoDaddy. `IMPLEMENTATION_GUIDE.md` covers how the cookie banner and privacy policy were set up.

## Authors

Built by Isaac David Moreno and Maxwell Sevilla Smith (BHS class of 2026). MIT licensed, see `LICENSE`.
