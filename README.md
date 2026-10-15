# dsc_rp_assets

Public image assets for **Stellar Presence**, a customised Discord Rich Presence for
[YouTube Music Desktop App](https://github.com/ytmdesktop/ytmdesktop). When a Hoshimachi Suisei song plays,
the presence keeps the song's own album cover as the large image and swaps the small playback badge for a
custom comet insignia. Every other artist keeps the app's normal badges.

Preview page: <https://justmino.github.io/dsc_rp_assets/>

## Assets

Base URL: `https://justmino.github.io/dsc_rp_assets/assets/badges/`

| File | Used for | Format |
|---|---|---|
| `suisei/comet-play.webp` | Suisei song playing (preferred) | 128 px animated WebP, full alpha, 3 s loop |
| `suisei/comet-play.gif` | Same, fallback if WebP does not animate | 128 px animated GIF, 1-bit transparency |
| `suisei/comet-play.png` | Same, still (animation disabled / static fallback) | 128 px PNG, alpha |
| `suisei/comet-pause.png` | Suisei song paused or buffering | 128 px PNG, alpha |

All badges have a transparent background and fit inside the circle Discord crops small images to.

## How they are served

GitHub Pages serves this repository from the `main` branch root (`.nojekyll` disables Jekyll processing), so
every file is available at `https://justmino.github.io/dsc_rp_assets/<path>`. Discord fetches the image once
through its media proxy (`media.discordapp.net/external/...`) and caches it.

Use the Pages URLs above. `github.com/.../blob/...` links are HTML pages, not images.

## Updating an asset

The badges are rendered offline by the generator in the Stellar Presence development project
(`stellar/generator/comet_anim.py`, checked by `stellar/generator/verify.py`). To update one:

1. Regenerate and verify it there.
2. Copy the file over the one in `assets/badges/suisei/`, keeping the file name.
3. Commit and push to `main`. Pages redeploys within a minute or two.

Discord's media proxy caches each external image URL for a year (`max-age=31536000`), so replacing a file under
the same URL does not reach Discord. After replacing a file, bump its version query in the app's theme config
(for example `comet-pause.png?v=2` -> `?v=3`). GitHub Pages ignores the query, but Discord treats it as a new
image and fetches it fresh. Current versions: `comet-play.webp` (v1, no query), `comet-pause.png?v=2`.
