# CS2 Suite — demo library

Pre-parsed CS2 matches for the **Library** tab of the CS2 Suite.

A CS2 demo is 200–450 MB, and every machine that opens one has to spend
20–60 seconds parsing it before anything can be drawn. That work only needs
doing once. This repository holds the *result* of that parse, so the app can
download a match in a couple of seconds instead.

Roughly **2 MB per match instead of ~300 MB** — about 130× smaller.

## What's here

| File | What it is |
|---|---|
| `index.json` | The catalogue: every published match, with teams, map, event, date and world rank |
| `teams.json` | Team → world rank, from Valve's published global standings |
| `events.json` | Match → tournament name |

The match data itself lives in the [`library` release](../../releases/tag/library),
two assets per match:

- `<match>.v2.cs2lib` — player positions, kills, bomb, grenades, inventory (~2 MB)
- `<match>.v2.cs2nades` — grenade trajectories only, for the Analyser (~0.3 MB)

Release assets are immutable and append-only: a published match is never
rewritten, only added to.

## Using it

You don't need this repository directly. Open the CS2 Suite, go to the
**Library** tab, filter by team, map, event, rank or date, and click Download.

## What the files contain

Derived positional data — coordinates, timings, and event records extracted
from public tournament demos. They are **not** playable demo files and cannot
be replayed in CS2, recorded from, or converted back into a `.dem`. Match
data is factual information about what happened in a public esports match.

Nothing here is produced by, endorsed by, or affiliated with Valve, HLTV, or
any tournament organiser.

## Credits

Team ranks come from Valve's published
[global standings](https://github.com/ValveSoftware/counter-strike_regional_standings).

Team logos are sourced from [Liquipedia](https://liquipedia.net/counterstrike/),
whose content is licensed [CC-BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/).
They are bundled with the app rather than requested from Liquipedia at runtime,
per their [API terms](https://liquipedia.net/api-terms-of-use). Each logo
remains the trademark of the organisation it represents and is used here only
to identify the team whose match a row refers to.

Map icons are Counter-Strike game assets and remain the property of Valve.

## Format

Both asset types are gzipped JSON — deliberately not pickle, which cannot be
safely read from a file downloaded over the internet. The `.v2` in each name
is the payload format version; the app only offers matches whose version it
understands, so an older build degrades to "needs an update" rather than
failing to parse.
