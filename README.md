# Lucky Dip

A single-page set of random pickers, built for choosing between friends. The main feature is **Space battle**: every name gets a ship, and the last ship flying wins. You send one link, and everyone who opens it watches the same fight.

Everything is in one file, `index.html`. There's no build step, no server code and nothing to install.

## What's in it

| Tab | What it does |
|---|---|
| Wheel | Spin a wheel of names. Can remove the winner after each spin. |
| Space battle | Asteroids-style free-for-all. Shareable, and plays identically for everyone. |
| Out of a hat | Draw names one at a time without putting them back. |
| Teams | Split the list into 2 to 20 teams of near-equal size. |
| Running order | Shuffle the list into a numbered order, with a copy button. |
| Dice | 1 to 10 dice, from d4 to d100. |
| Coin | Heads or tails, with a running tally and streak. |
| Number | Random numbers in a range, with an optional no-repeats setting. |

The Wheel, Space battle, Hat, Teams and Running order tabs all use the shared **Your list** box. Put one name per line.

Full details are in the docs:

- [docs/space-battle.md](docs/space-battle.md): game rules, how shared links work, the simulation spec
- [docs/tools.md](docs/tools.md): how each of the other pickers works
- [docs/hosting.md](docs/hosting.md): putting it online so people without Claude can watch

## Where it lives

- **Claude artifact:** https://claude.ai/artifact/5R3J9QHsX3Fiz61dL5TmCE. Private by default. Anyone opening a shared artifact needs a Claude account.
- **Standalone file:** `index.html` in this folder. Works on any static host. See [docs/hosting.md](docs/hosting.md).

## Default list

Rob, Sarah, Marc, Rosey, Donna + Das, Eric, Lucille, Helen, Luke, Angel, Dotty, Jamie, Alex

"Donna + Das" is a single entry, so they share one ship and one team slot.

The default list is set in the `SAMPLE` array in `index.html`. The "Reset to crew" button restores it.

## Saved settings

Each viewer's browser keeps its own settings in `localStorage`, under keys that start with `luckydip:`:

| Key | Holds |
|---|---|
| `list-v2` | The name list |
| `tab` | The last tab opened |
| `teams` | The number of teams |
| `dice` | Dice count and number of sides |
| `num` | Number picker settings |
| `battleSeed` | The current Space battle ID |
| `battleSpeed` | The chosen Space battle speed |

Nothing is sent to a server. If storage is blocked, as in some private windows, the page still works but forgets these between visits.
