<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/PasinduSuraweera/timeline/main/docs/banner-dark.svg" />
    <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/PasinduSuraweera/timeline/main/docs/banner-light.svg" />
    <img alt="A pixel wave drawn from a year of GitHub contributions, with a small surfer on the crest" src="https://raw.githubusercontent.com/PasinduSuraweera/timeline/main/docs/banner-dark.svg" width="880" />
  </picture>
</div>

<br>

<div align="center">

[![GitHub Marketplace][market-badge]][market-url]
[![Release][release-badge]][release-url]
[![Live site][site-badge]][site-url]
[![MIT license][license-badge]][license-url]

</div>

  [market-badge]: https://badgen.net/badge/marketplace/Timeline/ff6a3d?icon=github&labelColor=374151
  [market-url]: https://github.com/marketplace/actions/timeline-contribution-wave
  [release-badge]: https://badgen.net/github/release/PasinduSuraweera/timeline-action?labelColor=374151&color=ff6a3d
  [release-url]: https://github.com/PasinduSuraweera/timeline-action/releases
  [site-badge]: https://badgen.net/badge/site/timeline-readme.vercel.app/ff6a3d?labelColor=374151
  [site-url]: https://timeline-readme.vercel.app/
  [license-badge]: https://badgen.net/github/license/PasinduSuraweera/timeline-action?labelColor=374151&color=ff6a3d
  [license-url]: LICENSE

# Timeline Action

Draws your GitHub contribution year as an animated wave SVG and writes it into your repo, so your profile README can show it without calling anything when it loads.

The renderer lives in [PasinduSuraweera/timeline](https://github.com/PasinduSuraweera/timeline). If you only need the image, the hosted version at [timeline-readme.vercel.app](https://timeline-readme.vercel.app/) does the same with no workflow.

## Use it

Add this to your profile repo as `.github/workflows/timeline.yml`:

```yaml
name: timeline

on:
  schedule: [{ cron: "15 6 * * *" }]
  workflow_dispatch:

permissions:
  contents: write

jobs:
  timeline:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: PasinduSuraweera/timeline-action@v1
        with:
          user: YOUR_USERNAME
      - name: Commit the wave if it moved
        run: |
          git add -- assets/timeline.svg
          git diff --cached --quiet && exit 0
          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git commit -m "Update contribution wave"
          git push
```

Then put it in your README:

```
![my contribution wave](assets/timeline.svg)
```

[timeline-readme.vercel.app](https://timeline-readme.vercel.app/) writes this workflow for you with your settings filled in.

## Inputs

| Input | Default | What it sets |
| --- | --- | --- |
| `user` | required | GitHub username whose calendar is read |
| `out` | `assets/timeline.svg` | Where the SVG is written |
| `token` | `github.token` | Token used to read exact daily counts |
| `strict` | `false` | Fail rather than drawing a stand-in swell |
| `theme` | `ember` | `ember`, `ice`, `matrix`, `grape`, `rose`, `gold`, `mono`, `daylight` or `paper` |
| `rider` | `surfer` | `surfer`, `sail`, `buoy`, `flag` or `none` |
| `rider_at` | `33` | Where the rider sits, 0 to 100 percent |
| `width` | `880` | Band width in pixels, 320 to 1600 |
| `height` | `140` | Band height in pixels, 60 to 400 |
| `cell` | `4` | Size of one dot in pixels, 2 to 10 |
| `speed` | `33` | Seconds for one full scroll, 4 to 300 |
| `smooth` | `1.0` | Swell smoothing, 0.2 to 5 |
| `weeks` | `52` | Weeks of history to draw, 8 to 53 |
| `accent`, `bg`, `hot`, `glint` | theme | Hex colours for the accent, background, ridge cap and sparkle |
| `glints`, `dots`, `border` | `true`, `true`, `false` | Sparkle on the crest, dot grid behind the water, hairline border |
| `renderer` | `v1` | Tag or branch of the renderer to draw with |

---

Built by [PasinduSuraweera](https://github.com/PasinduSuraweera).
