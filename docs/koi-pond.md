# Koi pond

Every name floats on a round pond as a pellet of fish food. Koi swim around and eat the pellets one at a time. The last name left floating is the chosen one.

It looks like a game on a tiny LCD screen: a near-black background, flat mint-green lily pads with a notch cut out, drifting specks, small square food pellets and coloured koi. The pond is drawn at low resolution and scaled up with hard pixel edges, while the name labels stay sharp on top. It is a tall 9:16 rectangle (720 × 1280 world units, drawn at 180 × 320 pixels), sized for a phone held upright and pinned to the top of the screen. Names sit faintly to the right of their pellets. All controls and text (status line, last event, play and skip buttons, rules) sit in a quiet strip under the pond rather than on top of it.

## How to run one

1. Open the **Koi pond** tab. The pellets use the names in **Your list**.
2. Press **New pond** to get a new layout and a new outcome. Each pond has an ID, such as `#XEIKOQ`.
3. Pick a **Speed** (the default is 1×). The speed goes into the link.
4. Share it using **Send on WhatsApp** or **Copy link**.
5. Watch it using **Watch in full screen** or **Watch here**.

Anyone who opens the link gets the same watch-only page as Space battle: just the pond, the name tabs and a play button.

## What happens

- A rules screen comes up, followed by a 3, 2, 1, Go! countdown (1.2 s per number).
- **3 koi** for up to 6 names, **4** for up to 12, and **5** for more than 12. The koi are traditional varieties: Kohaku, Ogon, Showa, Asagi, Tancho and Chagoi.
- When a koi is ready to eat, it picks two random pellets, heads for the nearer one, and eats it on arrival. After eating, it rests for 4.5 to 10.5 seconds. Rests are 50% longer once 3 or fewer pellets are left, to build tension.
- Pellets drift slowly, and get nudged aside by passing fish.
- When only one pellet is left, it glows gold and the chosen one is announced, with confetti.
- The ticker and the Eaten list record who was eaten, by which koi, and in what order.
- 13 names usually takes about half a minute at 1×.

## Following your name

The tabs under the pond show each name as Floating, Eaten #n or Chosen. Tap your name to put an orange ring and a "(you)" label on your pellet. In full screen on a landscape screen, a side panel lists the names still floating.

## How everyone sees the same pond

It works the same way as Space battle (see [space-battle.md](space-battle.md)): a seeded random-number generator, fixed 1/120 s simulation steps, a sine/cosine lookup table and `Math.sqrt` only. The fish steer using a cross-product test rather than `atan2`, because `atan2` can give slightly different results in different browsers.

The lily pads and stones come from a second, separately seeded generator, so the scenery is identical for everyone without affecting the outcome. Swimming wiggle, ripples and confetti are visual only.

Link format:

```
<page address>#koi.<seed in base 36>.<names in base64url>.x<speed × 100>
```

Tested: the same link played on a phone-sized screen and a desktop-sized screen picked the same chosen one, at the same time.

## Where it is in the code

Everything is in `index.html`, inside `tools.koi`:

| Part | Function |
|---|---|
| Setup | `setup()` |
| One simulation step | `step()`, `eat()`, `steer()` |
| Drawing | `draw()`, `drawKoi()`, `bodyPath()`, `spineOf()` |
| Ending | `finish()` |
| Link parsing (shared with Space battle) | `parseLink()` |

## Fish introductions

After Play is pressed, and before the countdown, each koi gets a 2.6-second turn on a retro character-select screen:

- The fish is a simple low-poly 3D model (a tapered body with tail, dorsal and side fins, eyes and the fish's own colour pattern). It is rendered at 72 × 72 pixels with three flat shading tones, then scaled up with hard pixel edges.
- It pops in and rotates on a turntable above a green perspective grid.
- Its name ("the kohaku") sits above it, and a strange fact is typed out below like a terminal, for example "is legally three fish".
- **Skip intro** jumps straight to the countdown.

The fact each fish gets comes from the pond's seed, so everyone with the same link sees the same intro. The intro is visual only and doesn't change who gets eaten. Code: `buildModel()`, `renderModel()`, `startIntro()`, `drawIntro()`, `endIntro()`, `TRAITS`.

## Shared links

A shared pond link opens straight into a full-screen pond with a single Play button. There is no header, tabs, list or other controls.
