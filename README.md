# X Viral Monitor Lite

Tampermonkey userscript for X.com.

## Features

- Velocity badge on tweets, calculated from views per hour
- Bookmark count beside the bookmark button
- "Copy as Markdown" entry in the native X share menu

This lite version intentionally does not include the image viewer, long-image reader, leaderboard, star chart, or Grok reply tools.

## Install

Open this URL in a browser with Tampermonkey installed:

https://raw.githubusercontent.com/hahapkpk/x-viral-monitor-lite-userscript/main/x-viral-monitor-lite.user.js

If the original `X Viral Monitor` Chrome extension is enabled, disable it before testing this userscript to avoid duplicate badges and menu items. If another tool already injects its own "复制为 Markdown" item, this script takes that item over instead of adding a second one.

## Changelog

### 0.2.2

Adapted to X's XDS frontend rewrite (verified against the live site):

- DOM: every tweet anchor moved. `article[data-testid="tweet"]`, `[data-testid="User-Name"]`,
  `[data-testid="caret"]`, `button[data-testid="bookmark"]` and `[data-testid="tweetText"]`
  no longer exist. New anchors are `[data-engagement-action="share"]`, `.x-middot-group`,
  `button[aria-label="书签"]` and `.whitespace-pre-wrap.wrap-anywhere[lang]`.
- The `<time datetime>` element is gone; tweet age now comes from the permalink's relative
  text, or precisely from GraphQL.
- GraphQL endpoint moved from `x.com/i/api/graphql/` to `api.x.com/graphql/`.
- Tweet payload restructured: `legacy.created_at` → `details.created_at_ms` (epoch ms),
  `legacy.full_text` → `details.full_text`, engagement counters → `counts.*`,
  `legacy.entities.urls` → `url_entities`, media → `media_entities2`,
  and the author moved to `core.user_results.result.core`.
- Share menu: Base UI mounts an empty `[role=menu]` and fills items in a later commit, so
  injection now waits for the menu to populate instead of giving up on the first mutation.
- GraphQL data is authoritative; DOM scraping only fills fields the API did not supply.

### 0.1.0

Initial lite release.

## License

MIT. This script is a minimal userscript adaptation of the MIT-licensed `Icy-Cat/x-viral-monitor` feature set.
