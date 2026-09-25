# The other pickers

Every tool that picks at random uses the browser's cryptographic random-number generator (`crypto.getRandomValues`). It uses rejection sampling, which means discarding values that would favour some results, so every outcome is equally likely. Shuffles use Fisher–Yates, a standard method where every order is equally likely.

The exception is Space battle, which uses a seeded generator so it can be replayed exactly. See [space-battle.md](space-battle.md).

## Wheel

- The wheel has one segment per entry in Your list. The pointer is at the top.
- The winner is chosen first. The spin then eases out over 4.2 to 5.4 seconds and lands at a random point inside the winner's segment.
- **Remove winner after** deletes the first matching line from Your list once the spin finishes.
- You need at least two entries to spin.
- Segments use six colours in turn.

## Out of a hat

- Draws without replacement: once a name is drawn, it stays out until you press **Put them all back**.
- Duplicate lines count as separate slips.
- The drawn names are listed in the order they came out.

## Teams

- Shuffles the list, then deals names out one at a time, like cards, so team sizes differ by one at most.
- Allows 2 to 20 teams. You need at least as many names as teams.

## Running order

- Shuffles the list into a numbered order.
- **Copy list** copies it as `1. Name` lines. If copying is blocked, it selects the list so you can copy it yourself.

## Dice

- Rolls 1 to 10 dice with 4, 6, 8, 10, 12, 20 or 100 sides.
- d6 dice show pips. Other dice show the number.
- The total appears when you roll more than one die.

## Coin

- Flips with a 3D animation.
- Counts heads and tails, shows the current streak and the last 30 flips.
- **Reset tally** clears the counts.

## Number

- Picks up to 100 numbers from a range. The range includes both ends, and they can be entered either way round.
- **No repeats** stops a number coming up twice. If you ask for more numbers than the range holds, you get every number in the range, shuffled.

## Accessibility and motion

- Results are announced to screen readers as they appear.
- All controls work from the keyboard, with a visible focus outline.
- If a viewer's device is set to reduce motion, the spin, flip, dice shake and pop animations are skipped, and results appear straight away.
- Light and dark themes follow the viewer's system setting.
