# Space battle

The last ship flying wins. The ships fly and shoot at random, so nobody controls the outcome. Every ship gets the same stats.

## How to run one

1. Open the **Space battle** tab. The ships use the names in **Your list**.
2. Press **New battle** until you get a starting layout you like. Each battle has an ID, such as `#K7SVF`, shown in the corner of the arena.
3. Pick a **Speed**: 0.5×, 0.75× (the default), 1×, 2× or 4×. The speed goes into the link, so everyone watches at the pace you chose.
4. Share it using **Send on WhatsApp** or **Copy link**.
5. Watch it using **Watch in full screen** or **Watch here**.

Anyone who opens the link gets a watch-only page. It has no tabs, no list, no speed control and no share box: just the arena and a play button.

If you change the list, a new battle is created automatically. Send the new link, because the old one still has the old names in it.

## Rules

- Each ship starts with **3 shields**. Losing all three knocks it out.
- A bullet hit costs 1 shield. A ship can't be hit by its own bullets.
- Flying into an asteroid costs 1 shield and bounces the ship away.
- Shooting a large asteroid splits it in two. Shooting a small one destroys it.
- The fire rate rises steadily over time, so a battle can't stall. The on-screen counter says "firing faster" once 25 seconds have passed.
- The last ship left wins. If the final two are knocked out in the same instant, the one knocked out last wins.
- The knockout feed records who took out whom, in finishing order (#13 is out first, #2 is the runner-up).

A battle with 13 pilots usually lasts well under two minutes at 1×. At the default 0.75× it takes a third longer.

## How everyone sees the same fight

The fight isn't streamed. Each viewer's browser runs the whole battle itself from the same starting numbers. The simulation is deterministic, meaning the same inputs always give the same fight, so every viewer gets the same shots, knockouts and winner.

The inputs are:

1. **Seed**: a random whole number, chosen when you press New battle. It is shown as the battle ID.
2. **Names**: the list, in its original order.
3. **Speed**: how fast it plays. This changes the pace only, never the outcome.

All three go in the link:

```
<page address>#fight.<seed in base 36>.<names in base64url>.x<speed × 100>
```

For example: `…#fight.k7svf.Um9iClNhcmFo….x075` plays at 0.75×.

- The speed part is optional. Links sent before it existed play at 0.75×.
- Only 0.5, 0.75, 1, 2 and 4 are accepted. Anything else falls back to 0.75×.

- The names are joined with line breaks, encoded as UTF-8, then base64url-encoded with `=` padding removed. That keeps the link to characters Claude artifact links allow in the anchor (letters, digits, `.`, `_`, `-`).
- When the page opens with a `#fight.` link, it goes straight into watch-only mode.
- The names can be decoded by anyone who has the link. Only put in names you're happy to share with whoever receives it.

### What keeps it deterministic

| Rule | Why |
|---|---|
| All game decisions use a seeded random-number generator (mulberry32) | Same seed, same sequence of choices |
| The simulation advances in fixed steps of 1/120 s | Frame rate and device speed don't change the result |
| Speed (0.5× to 4×) only changes how many steps run per second | Pace changes, the outcome doesn't |
| Headings use a 1,024-step sine/cosine table rounded to 9 decimal places | Removes tiny differences between browsers' maths libraries |
| Distances use only `Math.sqrt`, and drag is a fixed constant | Both give identical results in every browser |
| Explosions, stars and engine flicker use `Math.random` | These are visual only and never affect the fight |
| The world is always 1000 × 625 units | Screen size can't change positions |

Tested: the same link played in two separate Chromium windows gave the same winner, knockout count and duration. Safari and Firefox haven't been compared side by side yet.

## Simulation reference

All distances are in world units. The arena is 1000 × 625, and it wraps at the edges as in Asteroids.

### Start

- The names are shuffled with the seed and spaced evenly around an ellipse at 36% of the arena's width and height, starting at a random angle.
- Each ship faces a random direction.
- Ships can't be damaged for the first 1 s.
- Four asteroids start near the centre, with a radius of 30 to 50.
- A ship's colour comes from its position in the original list, cycling through six colours.

### Ships

| Setting | Value |
|---|---|
| Turn rate | Random, up to ±520 heading units/s (1,024 units per full turn, about ±3.2 rad/s) |
| Turn change | Every 0.25 to 1.35 s |
| Thrust | About 1.4 bursts per second, each lasting 0.25 to 0.95 s |
| Acceleration | 260 units/s² while thrusting |
| Drag | Speed × 0.9924 every step |
| Top speed | 240 units/s |
| Time between shots | (0.35 to 1.65 s) ÷ (1 + seconds elapsed ÷ 25) |
| Hit radius | 19 |
| Protection after a bullet hit | 0.35 s |
| Protection after an asteroid hit | 0.9 s |

### Bullets

| Setting | Value |
|---|---|
| Speed | 480, plus 40% of the ship's velocity |
| Spread | ±10 heading units (about ±3.5°) |
| Lifetime | 1.05 s |

### Asteroids

| Setting | Value |
|---|---|
| Collision radius against ships | Asteroid radius + 12 |
| Split | Radius over 22 splits into two at 0.58× the size, moving 1.8× faster |
| Target number on screen | min(8, 3 + ⌊seconds ÷ 20⌋) |
| New asteroids | Enter from a random edge, radius 28 to 50, about 0.8 per second while below the target |

## Full screen

- **Watch in full screen** asks the browser for real full screen, and tries to lock phones to landscape.
- Where full screen isn't allowed, the arena fills the window instead. This is the `.theatre` class, and it happens on iPhones and inside Claude's viewer.
- In portrait, a hint suggests turning the phone sideways. The arena is always 16:10.
- To leave full screen, use **Exit full screen** or press Escape.

## Known limits

- Viewers don't watch at the same moment. Each person's fight starts when they press play. To watch together, agree a time and all press play then.
- iPhone Safari doesn't support true full screen for web pages, so the arena fills the window instead.
- A Claude artifact link needs a Claude account to open. To reach people without Claude, host `index.html` yourself (see [hosting.md](hosting.md)).

## Where it is in the code

Everything is in `index.html`, inside `tools.battle`:

| Part | Function or constant |
|---|---|
| Link encode and decode | `encNames`, `decNames`, `parseFight`, `speedCode` |
| Speed options | `SPEEDS`, `DEFAULT_SPEED` |
| Share link base | `ART_URL`: the artifact URL, or the page's own address when `window.LUCKY_STANDALONE` is true |
| Random numbers | `mulberry` |
| Setup | `setup()` |
| One simulation step | `step()` |
| Drawing | `draw()` |
| Frame loop | `loop()` |
| Full screen | `enterFull()`, `exitFull()` |

## Ideas for later

- A countdown to a set start time in the link, so everyone watches live together.
- Sound effects, started from the play button.
- Team battles, using the Teams tab.
- A replay slider to scrub back to a knockout.
