# Student Life OS

Local-first student planner: tasks, calendar, exams, reminders, focus timer, analytics. React + TypeScript + Vite + Tailwind + Recharts + Lucide.

## Run locally
```
npm install
npm run dev        # http://localhost:5173
npm run build      # type-check + production build into dist/
```

## Deploy to GitHub Pages
1. Create a GitHub repo and upload ALL files from this zip (unzipped), including the hidden `.github` folder. Or: `git init && git add . && git commit -m "init" && git branch -M main && git remote add origin <url> && git push -u origin main`.
2. In the repo: **Settings > Pages > Source: GitHub Actions**.
3. Push to `main`. The included workflow (`.github/workflows/deploy.yml`) builds and publishes. Your site appears at `https://<user>.github.io/<repo>/`.

No base-path change is needed: `vite.config.ts` uses `base: './'` and the app uses hash routes (`#/tasks`).

## Folder structure
```
src/App.tsx          layout, navigation, reminder engine
src/store.ts         data model, localStorage persistence, actions
src/stats.ts         analytics calculations
src/util.ts          dates, toast, notifications
src/components.tsx   modal, task/event forms, rows
src/pages/           Dashboard, Tasks, Calendar, Exams, Planner, Analytics, Settings
```

## Storage and backups
Everything is saved in your browser's localStorage on this device only. It does not sync between devices or browsers, and clearing site data erases it. Use Settings > Export JSON regularly; Import JSON validates the file and asks before replacing data. All data lives in one object in `store.ts`, so adding cloud sync later means swapping `setDB`/`load` for API calls.

## Reminders: limits
Reminders are checked every 15 seconds by the open page. They show in the bell panel and as browser notifications if you grant permission (asked only when you click the button). If the tab or browser is closed, nothing fires until you reopen the site. Each reminder notifies once; Dismiss and Snooze are supported.
For real background alerts you need: a service worker + Web Push (VAPID keys), a small backend or serverless function that stores subscriptions and sends pushes on schedule (e.g. Supabase/Firebase Cloud Messaging), and user accounts. Not included in this version.

## Feature checklist
Implemented: dashboard with live stats/charts/quick add/focus/countdowns/activity; tasks (create, edit, duplicate, delete, complete/reopen, subtasks, priority, status, estimate, daily/weekly repeat, reminder, filters, search, sort, subject filter); month and week calendar with events; exams list with countdowns and 3-day highlight; duplicate/invalid event checks; in-app reminders with snooze/dismiss and browser notifications; Pomodoro timer (persisted, subject/task/exam link) and study-plan blocks; analytics (daily, weekly/monthly trend, completed vs pending, on-time vs late, subjects, study time, heatmap, streaks); settings (theme, subjects, time format, week start, default reminder, timer lengths), JSON export/import, delete-all; light/dark mode.
Verified: TypeScript type-check and production build pass. I did not click through the UI in a browser, so test the main flows after deploying and report any bug.
Not included: sample data, Google/ICS calendar sync, cloud sync, push notifications.
