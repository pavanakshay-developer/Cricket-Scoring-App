# Cricket Scoring App

A professional single-file HTML5/JavaScript cricket scoring app for T20 match scoring with Supabase-backed save/load. It is designed for phone-first use on the boundary, while still working well on desktop browsers.

## Supabase Setup

The app is still a single static `index.html` file, but match scorecards now save to Supabase instead of permanent browser storage.

1. Create a Supabase project.
2. Run this SQL in the Supabase SQL editor:

```sql
create table public.matches (
  id uuid primary key default gen_random_uuid(),
  title text not null,
  match_data jsonb not null,
  complete boolean not null default false,
  updated_at timestamptz not null default now()
);

create or replace function public.set_updated_at()
returns trigger
language plpgsql
as $$
begin
  new.updated_at = now();
  return new;
end;
$$;

create trigger set_matches_updated_at
before update on public.matches
for each row
execute function public.set_updated_at();

alter table public.matches enable row level security;

create policy "Public read matches"
on public.matches for select
to anon
using (true);

create policy "Public create matches"
on public.matches for insert
to anon
with check (true);

create policy "Public update matches"
on public.matches for update
to anon
using (true)
with check (true);
```

3. In `index.html`, set `SUPABASE_PUBLISHABLE_KEY` near the top of the script. Do not put a Supabase secret key in this static HTML file.

If the app shows `Create matches table` or Supabase returns `PGRST205`, the SQL above has not been run in the connected project, or Supabase needs a moment to refresh its schema cache.

## Live Hosting

This project is ready for basic GitHub Pages hosting. Because the app is contained in `index.html` and loads Supabase from a CDN, GitHub Pages can serve it directly from the repository root.

## Features

- Match setup for two teams with full player rosters.
- T20 scoring with 20 overs per innings.
- Phone-focused scoring dashboard.
- Light and dark theme toggle with local preference saving.
- Dynamic striker and non-striker display.
- Manual `Swap Strike` override.
- Bowler tracking and change bowler controls.
- Automatic new bowler prompt after every completed over.
- Ball-by-ball log shown as compact score tokens.
- Editable ball history.
- Replay-based re-simulation after edits to preserve score, strike, bowler figures, and scorecard integrity.
- Wicket workflow with dismissal type and fielder capture where needed.
- Automatic next-batter prompt after a wicket.
- Full scorecard modal with current and previous innings views.
- Printable match overview and innings scorecards.
- Supabase match storage with multiple saved matches.
- Lightweight pending-sync fallback if the scorer loses internet.

## Cricket Logic

- Wides and no-balls add a 1-run penalty.
- No-ball triggers a free hit indicator.
- Free hit protects against bowled, caught, and LBW dismissals.
- 2022 caught-dismissal rule is supported: the new batter takes strike after a catch.
- Strike rotates on odd runs and at the end of legal overs.
- Bowling limit is 4 overs per bowler.
- CRR, RRR, strike rate, and economy rate are calculated live.

## Offline Use

Open `index.html` directly in any modern browser. An internet connection is required for Supabase sync, but the app keeps the latest unsynced scorecard snapshot in browser storage if a save fails and pushes it when the connection returns.

## GitHub Pages Setup

1. Push this repository to GitHub.
2. Open the repository settings.
3. Go to `Pages`.
4. Choose the `main` branch and repository root.
5. Save the settings.

The app should then be available from the GitHub Pages URL for this repository.

## Local Development

No build step is required.

```powershell
git clone https://github.com/pavanakshay-developer/Cricket-Scoring-App.git
cd Cricket-Scoring-App
```

Then open `index.html` in a browser.

## Commit Workflow

```powershell
git status
git add index.html README.md
git commit -m "feat: update cricket scoring app"
git push
```

## File Structure

```text
.
|-- .github/
|-- index.html
`-- README.md
```

## Notes

- The app intentionally stays as one HTML file.
- Match data is saved in Supabase.
- Browser storage is only used for theme preference, the active match ID, and pending sync recovery.
- Historical edits trigger full replay of the innings event log.
