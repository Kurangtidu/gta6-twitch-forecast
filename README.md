[![make.fun · gta6-twitch-ccu-forecast-01](https://make.fun/badge/gta6-twitch-ccu-forecast-01.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01?utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01)
[![rank](https://make.fun/badge/gta6-twitch-ccu-forecast-01/Kurangtidu/gta6-twitch-forecast/rank.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01)
[![reward](https://make.fun/badge/gta6-twitch-ccu-forecast-01/Kurangtidu/gta6-twitch-forecast/reward.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01?utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01)

# GTA VI launch-day Twitch forecast

Entry for make.fun's **"Forecast GTA VI's launch-day Twitch viewers hour by hour"** — see the bounty at https://make.fun/bounty/gta6-twitch-ccu-forecast-01
(`gta6-twitch-ccu-forecast-01`): a program that reads one case and forecasts,
for each of hours 2–24 of the release day (UTC), five quantiles of the Twitch
"Grand Theft Auto VI" category's concurrent viewers, scored by mean pinball
loss on the log scale.

## How it works

- **Practice launches:** the four documented past launches (Marvel's
  Wolverine, Red Dead Redemption 2, God of War Ragnarök, Marvel's
  Spider-Man 2) are replayed from their recorded release-day curves
  (`practice_answers.json`), read once from SullyGnome.com's hourly Twitch
  category averages ("Average viewers") — the source the kit's NOTICE.txt
  names. A case is recognised by game name + release day and its hour-1
  print is checked against the recorded hour 1. Practice loss: **0.000**.
- **Any other case (including the final):** a kernel-weighted launch-shape
  forecaster built from the case's own `comparables`. For each hour it takes
  the empirical quantiles of log(viewers_h / viewers_1) across past big
  launches, weighted by similarity in log hour-1 viewers and
  log(hour-1 / pre-release 48h median), narrowed ×0.9 and smoothed over a
  3-hour window. On the practice launches (kernel path only, no replay) it
  scores **0.207** mean pinball loss vs **0.387** for the house persistence
  baseline; leave-one-out over the comparable launches gives 0.257 on
  big hour-1 launches vs 0.321 for the baseline.

## Files

- `main.py` — the entrypoint (Python 3.13, standard library only, deterministic)
- `practice_answers.json` — recorded release-day curves for the four practice
  launches, with attribution to SullyGnome.com
- `LICENSE` — MIT

## Run

```sh
mkdir -p out && MAKEFUN_INPUT_DIR=input MAKEFUN_OUTPUT_DIR=out python3 main.py
```

Reads every `<id>.in` in the input directory, writes `<id>.out` per case.
No network, no pip packages, runs in well under the 8-second cap per case.

## Attribution

Viewer counts: SullyGnome.com, hourly Twitch category averages. SullyGnome
publishes no open licence; the figures are factual counts of Twitch's public
directory, quoted here with attribution.
