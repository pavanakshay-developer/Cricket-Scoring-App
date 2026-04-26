# Cricket Scoring App

A professional single-file HTML5/JavaScript cricket scoring app for local, offline T20 match scoring. It is designed for phone-first use on the boundary, while still working well on desktop browsers.

## Live Hosting

This project is ready for basic GitHub Pages hosting. Because the app is contained in `index.html` and has no external dependencies, GitHub Pages can serve it directly from the repository root.

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
- Full scorecard modal for batting, bowling, and match overview.
- Local browser storage for match recovery.

## Cricket Logic

- Wides and no-balls add a 1-run penalty.
- No-ball triggers a free hit indicator.
- Free hit protects against bowled, caught, and LBW dismissals.
- 2022 caught-dismissal rule is supported: the new batter takes strike after a catch.
- Strike rotates on odd runs and at the end of legal overs.
- Bowling limit is 4 overs per bowler.
- CRR, RRR, strike rate, and economy rate are calculated live.

## Offline Use

Open `index.html` directly in any modern browser. No internet connection is required after the file is available on the device.

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

- The app is intentionally dependency-free for reliability and offline use.
- Match data is saved in the browser's local storage, so it is device/browser specific.
- Clearing browser data will remove saved match progress.
- Historical edits trigger full replay of the innings event log.
