# BHS Bell

A schedule tracker for Barcelona High School students. Open it and see, at a glance, what day it is, what period you're in, how long until it ends, and what's next.

**Live site:** [bhsbell.com](https://bhsbell.com)

Built by two students, Isaac David and Max Smith, during the 2025-26 school year. Currently updated for the 2026-27 school year.

## Features

- **Live period tracking.** Shows the current period, how many minutes are left, and what comes next. Updates every minute.
- **Holiday and weekend handling.** On days off it shows the holiday name and tells you the next school day.
- **Special schedules.** Half days, assemblies, parties and other one-off timetables override the regular schedule and turn the page orange.
- **Calendar.** The school's monthly calendar, opening on the current month.
- **Countdown.** School days remaining until the last day of the year, with a progress bar. Weekends and holidays are skipped.
- **Lobo.** Archive of the student newspaper, *The Lobo*, as embedded PDFs.
- **About and contact pages.** Info about the developers and a way to send feedback.
- **Privacy policy and cookie notice.** GDPR-oriented, with an accept/decline banner.
- **Mobile-first.** Responsive layout with a hamburger menu on small screens.

## Tech stack

Plain HTML, CSS and JavaScript. There is no framework, no build step, no backend and no dependencies. All schedule logic runs in the browser.

| Part | Details |
| --- | --- |
| Hosting | Netlify (auto-deploys on push to `main`) |
| Domain | GoDaddy |
| Data | A single JSON file, `scheduleData.json` |
| Calendar | Embedded Google Slides deck |
| Storage | `localStorage` only, to remember the cookie banner choice |

## Project structure

```
index.html / script.js          Home page: live day and period tracker
calender.html / .js / .css      Monthly calendar (Google Slides embed)
countdown.html / .js / .css     School-days-left countdown
lobo.html / lobo.css + lobo/    Student newspaper PDF archive
about.html / about.css          About the developers
contact.html / contact.js       Contact links (email and WhatsApp)
privacy.html / privacy.css      Privacy policy
cookie-consent.js / .css        Cookie banner
scheduleData.json               All schedule data (see below)
style.css                       Shared styles
schedule/                       Monthly calendar images
IMPLEMENTATION_GUIDE.md         Notes from adding the cookie banner and privacy policy
```

## Running it locally

There is nothing to install. `scheduleData.json` is loaded with `fetch`, so opening `index.html` directly from disk will fail in most browsers. Serve the folder instead:

```bash
git clone https://github.com/isaacdavidm/bhs-schedule-tracker.git
cd bhs-schedule-tracker
python3 -m http.server 8000
```

Then open http://localhost:8000.

## Updating the schedule

Each new school year only needs edits to `scheduleData.json`:

- **`school_info`**: school name and academic year.
- **`schedule.period_times`**: start and end times (`HH:MM`, 24-hour) for each period, passing period, homeroom and lunch on a regular day.
- **`holidays`**: one entry per day off, with a `date` (`YYYY-MM-DD`), a `name`, and a `type` (`federal_holiday` or `school_holiday`). Multi-day breaks need one entry per day.
- **`special_schedules`**: one entry per unusual day. Each has a `date`, a `name`, a `type`, and a `schedule_override.period_times` block with the same shape as the regular schedule. Custom period names such as `party` or `concert` need a matching label in `script.js`.

Two other places contain hardcoded dates and need a manual update each year:

- `countdown.js`: the last day of school and the school year start date.
- `calender.js`: the month-to-slide mapping, and the Google Slides embed URL in `calender.html` if the deck changes.

## Privacy

The site has no analytics, trackers, accounts or forms, and doesn't collect personal data itself. Netlify and GoDaddy may log technical data such as IP addresses. If you add analytics, forms or accounts, update `privacy.html` and the cookie banner text to match. See [`privacy.html`](privacy.html) for the full policy.

## License

[MIT](LICENSE)
