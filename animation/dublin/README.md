# Dublin scenario (ProB animation of M4 with context C4_Dublin)

Runway 28L in mixed mode, Point Merge system with legs LEG_N and LEG_S and merge point LAPMO (also the common point).
ProB checked the invariants at every step: no violation was reported.

- `steps/` : screenshots taken during the animation, numbered in the order in which they were taken. During the animation, some steps were undone with the ProB history (back button) and replayed differently; the screenshots of the final scenario are those listed in the tables below. Not every step of the final scenario was captured.
- `history/` : the **exact trace** of the final scenario, from the ProB *History* view (read each image from the bottom to the top; the files are numbered in chronological order, with the time interval in the name). Each row is one event with its parameters; columns M0 to M4 show the machines in which the event exists (e.g. Direct_To, Entry_Leg and tick_tock appear from M2). The history contains only the final branch: the steps undone with the back button are not in it.
- `critical_states/` : the screenshots that show the critical states (CS), named `CSx_event_aircraft_time.png`.
- `fig_scenario_dublin.png` : timeline of the scenario (landings, take-offs, Point Merge events and critical states).

Values used: Separation_time (Medium→Light 131, Light→Medium 69, Medium→Medium 69, Heavy→Medium 157, Heavy→Light 196, Light→Light 82),
MergeBuffer = 10, FinalDuration = 240, RwySep = 60, DepDepSep = 60 (120 behind Heavy).

## Critical states

| CS | Meaning | t (s) | Event (aircraft) | Values in ProB | Screenshots |
|---|---|---|---|---|---|
| CS1 | Two aircraft at the same TMA entry point are too close: the second entry is delayed | 0 | Entry_in_TMA (A3, OLAPO) | A4 (Medium) enters at 0; A3 (Light) is delayed to 0 + 131 = 131; then A2 (Medium) to 131 + 69 = 200. At OSGAR, A8 (Light) is delayed to 0 + 196 behind A9 (Heavy) | `CS1_*.png` (steps 4, 6, 12) |
| CS2 | Two aircraft enter the same leg too close: the leg entry is delayed | 919 | Entry_Leg (A1, LEG_S) | ETA(A1) = 890 < 858 + 157 (A9, Heavy): leg entry delayed to 1015 | `CS2_*.png` (steps 40, 53) |
| CS3 | The direct-to would create a conflict at the merge point: it is refused | 1246 | Direct_To (A3) disabled | 1246 + 243 = 1489 < 1382 + 131 + 10 = 1523 (grd7) | `CS3_CS4_*.png` (step 64) |
| CS4 | The direct-to is refused: the aircraft moves to the next waypoint | 1246 | Move_On_Leg (A3) | 1246 ≥ 1114 + 93 (grd6); legPointTime(A3) = 1207 | steps 64, 66 |
| CS5 | The spacing is exactly the minimum: the direct-to is accepted | 1280 | Direct_To (A3) | 1280 + 243 = 1523 = 1382 + 131 + 10; mergeTime(A3) = 1523 | steps 70, 72 |
| CS6 | An aircraft would overtake another one: its scheduling is refused | 1381 | Schedule_Arrival (A8) disabled | after Direct_To (A8), Schedule_Arrival is enabled only for A3, which passes LAPMO first (1523 < 1649) (grd7 of Schedule_Arrival) | step 77 |
| CS7 | Landing clearance: the separation with the previous landing holds | 1114 | Landing_Verification (A4) | Threshold(A4) = 1114 + 240 = 1354 ≥ 1161 + 69 (grd11) | steps 56, 58 |
| CS7 | (second example) | 1585 | Landing_Verification (A1) | 1585 + 240 = 1825 ≥ 1621 + 157 (behind A9, Heavy); refused at 1435 (1675 < 1778) | steps 86, 87 |
| CS8 | Landing: the runway becomes unavailable, lights ON | 686 | Touchdown (A5) | RWY28L Unavailable, ON; lastTouchdown = 686 (also A10 at 1188) | steps 32, 63 |
| CS9 | A take-off too close to a cleared landing is refused | 1114 | Start_takingoff (A7) disabled | Threshold(A10) = 1161 < 1114 + 60 (grd7) | step 55 |
| CS9 | (after a landing) | 1246 | Start_takingoff (A7) disabled | 1246 < lastTouchdown 1188 + 60 = 1248 (grd6) | step 69 |
| CS9 | (no gap between landings) | 2008 | Start_takingoff (A1, A9) disabled | Threshold(A3) = 2028 < 2008 + 60 (grd7) | step 100 |
| CS10 | A gap exists between two arrivals: the take-off is performed in this gap | 1280 | Start_takingoff (A7) | 1280 ≥ 1188 + 60 and 1354 ≥ 1280 + 60 | steps 70, 71 |
| CS10 | (other examples) | 1503, 1907 | Start_takingoff (A10), (A4) | 1503 ≥ 1381 + 60, 1621 ≥ 1503 + 60; 1907 ≥ 1829 + 60, 2028 ≥ 1907 + 60 | steps 83, 94, 95 |
| CS12 | Two successive take-offs: the second one waits DepDepSep | 2475 | Start_takingoff (A9) disabled | 2475 < 2454 + 60 = 2514 (grd5), after the take-off of A1 at 2454 | steps 108, 109 |

CS11 (two merge points feeding the same common point) only exists at Oslo.

## Main instants

| t (s) | Event |
|---|---|
| 0–200 | Entry_in_TMA: A4, A3 (131), A2 (200) at OLAPO; A9, A8 (196) at OSGAR; A5, A7 at BOYNE/NIMAT; A1, A10 (57) |
| 187 | Entry_Leg and Direct_To (A5), merge 430 |
| 305 | Entry_Leg and Direct_To (A7), merge 548 |
| 438 | Landing_Verification (A5), Threshold 678 |
| 604 | Entry_Leg and Direct_To (A10), merge 872; Landing_Verification (A7), Threshold 844 |
| 686 | Touchdown (A5) |
| 858 | Touchdown (A7); Direct_To (A4), merge 1101 |
| 919 | Direct_To (A9), merge 1189 |
| 921 | Landing_Verification (A10), Threshold 1161; Start_takingoff (A5) |
| 1015 | Entry_Leg (A1) |
| 1114 | Landing_Verification (A4), Threshold 1354; Entry_Leg (A3); Direct_To (A1), merge 1382 |
| 1146 | Entry_Leg (A8) |
| 1188 | Touchdown (A10) |
| 1246 | Move_On_Leg (A3) |
| 1280 | Start_takingoff (A7); Direct_To (A3), merge 1523 |
| 1334 | Entry_Leg (A2); Move_On_Leg (A8) |
| 1381 | Direct_To (A8), merge 1649; Schedule_Arrival (A3) then (A8); Touchdown (A4) |
| 1503 | Start_takingoff (A10); A2 reaches LAPMO at 1746 |
| 1585 | Landing_Verification (A1), Threshold 1825 |
| 1657 | Touchdown (A9) |
| 1788 | Landing_Verification (A3), Threshold 2028 |
| 1829 | Touchdown (A1) |
| 1907 | Start_takingoff (A4); Landing_Verification (A8), Threshold 2147 |
| 2008 | Landing_Verification (A2), Threshold 2248 |
| 2059 | Touchdown (A3) |
| 2192 | Touchdown (A8) |
| 2354 | Touchdown (A2) |
| 2454 | Start_takingoff (A1) |
| 2475 | Start_takingoff (A9) refused: DepDepSep (CS12) |
| 2514 | Start_takingoff (A9) |
| 2562 | Terminating_takingoff (A9), Departing (A9): end of the scenario, every aircraft has completed its cycle |

At the end, every arrival has landed (TerminatedL) and the departures A5, A7, A10, A4, A1 and A9 have taken off (A9 starts its take-off at 2514).
Landed aircraft remain in aircrafts_in_TMA, since ground operations are not modelled.

## Remarks

- Event-B has no notion of urgency: tick_tock may let time pass after a threshold time, so some touchdowns occur after their planned time (e.g. A10 at 1188 for 1161). The separations are guaranteed when the touchdown occurs at its threshold time.
- ProB colours in red the first guard for which it finds no parameter value (e.g. `duration_final = FinalDuration(...)` in Landing_Verification); the guard that actually fails can be further down the list.
- Times: `Take-off` is the instant of Start_takingoff (start of the take-off roll, variable depTime); `Touchdown` is the instant of the event Touchdown; `Threshold` is the planned time computed by Landing_Verification (current time + FinalDuration).
- Several aircraft may be ready to take off at the same time (A5 and A7 at 921, A1 and A9 at 2008): grd4 of Start_takingoff allows ties.

## Additional part (after the scenario)

After the end of the scenario (t = 2562), the landed aircraft A8, A3 and A2 were cleared for take-off again, only to check DepDepSep behind a Heavy (files `history_09_*_extra.png` and `history_10_*_extra.png`). This is not an operational situation (ground operations are not modelled):

| t (s) | Event | Check |
|---|---|---|
| 2654 | Start_takingoff (A8, Light) | 2654 ≥ 2514 + 120 (behind A9, Heavy) |
| 2719 | Start_takingoff (A3, Light) | 2719 ≥ 2654 + 60 |
| 2826 | Start_takingoff (A2, Medium) | 2826 ≥ 2719 + 60 |
