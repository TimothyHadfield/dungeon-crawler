# Abyssal Depths: progress

## START HERE
Last updated 2026-10-16. Read order: this file, then `CLAUDE.md` (file map, how to run and test), then `README.md`.

## Goal right now
None set. The game is finished and published; work is upkeep only.

## Status
| Area | State |
|---|---|
| Game (10 floors, 4 classes, dragon boss) | Done, live |
| Phone play (d-pad) | Works; inventory/potions not reachable on phone (see Traps) |
| README showcase + screenshots | Done (2026-09-27) |
| Bug sweep | 2026-10-16: 5 fixes (below), pushed to main |

## Authorized next steps
None.

## Standing instructions
- Tim's global rules apply (`~/.claude/CLAUDE.md`). No project-specific quotes from Tim recorded yet.

## Traps
- No accounts and no backend; scores and achievements live in the browser's localStorage only.
- Phone width (768px or less) hides the side panel, so there's no inventory, minimap or potion use on a phone.
- iOS Safari: `body` is `100vh`, which is taller than the visible area when the toolbars show; the bottom message log may sit under the toolbar on a real iPhone (not checked on a device).
- More in `CLAUDE.md` Traps.

## Decisions
- 2026-10-16 sweep: fixed Ranger "+1 item per floor" missing on floor 1 (and leaking into the next run's class); message log carried over from the previous run; Warrior Berserker bonus lagging one turn; class picker cut off at the top on phones (now scrolls); title "ABYSSAL DEPTHS" left-aligned and touching the edge when it wraps on phones (now centred). Added meta description and aria-labels. No look changes on desktop.

## NOT verified
- Real iPhone: bottom of the game screen under Safari's toolbar (phone-view's WebKit uses a 659px viewport, where it fits).
- A full human playthrough after the 2026-10-16 fixes (verified by scripted runs: 6 bot runs, ~100k turns, all floors and a dragon kill, no errors).

## Rejected / parked
- (none)
