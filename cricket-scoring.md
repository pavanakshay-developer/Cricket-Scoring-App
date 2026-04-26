# Technical Specification for Cricket Scoring Application

**1\. Match Configuration and Regulations**

- **Format:** T20 ($20$ overs per innings).

- **Powerplay:** The first $6$ overs involve fielding restrictions ($2$ players permitted outside the $30$-yard circle).

- **Bowling Limits:** Maximum of $4$ overs per bowler.

- **Extra Runs:** Penalty for Wide (Wd) and No-Ball (Nb) is $1$ run plus any runs scored off that delivery.

- **Free Hit:** Any No-Ball must be followed by a Free Hit delivery where the batter cannot be dismissed by "Bowled," "Caught," or "LBW".

- **Strike Rotation Law:** Per 2022 ICC/MCC rules, a new batter always takes the strike after a "Caught" dismissal, regardless of whether the batters crossed.

**2\. Data Structures**

- **Player Object:** `{ id, name, runs, ballsFaced, fours, sixes, isOut, dismissalType, bowlerStats: { overs, maidens, runsConceded, wickets, wides, noBalls } }`.

- **Ball Event:** `{ ballNum, strikerId, bowlerId, runsBat, extraType, extraRuns, isWicket, wicketType }`.

- **Match State:** Recursive calculation where the state at $Ball_{n}$ is derived from the initial state plus the sequence of all preceding events.

**3\. State Management and Editing Logic**

- **Historical Editing:** The application must maintain a chronological log of "Ball Events".

- **Re-Simulation:** When an event is edited or deleted, the system must "rewind" to the point of change and "replay" all subsequent balls to correctly recalculate cumulative statistics like strike rotation, bowler spells, and team totals.

- **Manual Overrides:** The UI must allow the user to manually "Swap Strike" or "Change Bowler" mid-over or between overs to account for umpire corrections or injuries.

**4\. Mathematical Projections**

- **Run Rates:** Current Run Rate ($CRR = \frac{Runs}{Overs}$) and Required Run Rate ($RRR = \frac{Runs Needed}{Overs Remaining}$).

- **Amateur DLS:** If rain occurs, par scores are calculated using a $G_{50}$ (average 50-over total) of $200$.

- **Revised Target (T):** When $R_2 < R_1$, $T = \lfloor \frac{S \times R_2}{R_1} \rfloor + 1$. When $R_2 > R_1$, $T = S + \frac{(R_2 - R_1) \times G_{50}}{100} + 1$.
