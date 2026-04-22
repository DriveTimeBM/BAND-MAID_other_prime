# BAND-MAID_other_prime

A searchable, filterable archive of all BAND-MAID Prime videos **except live OKYUJI shows** (which live in the [BAND-MAID_live](https://github.com/DriveTimeBM/BAND-MAID_live) repo). Covers BTS footage, self-cover videos, Stationhead archives, special movies, instrumental MVs, unboxings, and more.

## Live

Hosted at: `https://drivetimebm.github.io/BAND-MAID_other_prime/`

## Data sources

1. **`prime.json`** — `https://drivetimebm.github.io/BAND-MAID_gpt/prime/prime.json`
   All BAND-MAID Prime videos. This page filters to `Category !== "OKYUJI"`.
2. **`setlists.json`** — `https://drivetimebm.github.io/BAND-MAID_prime/data/setlists.json`
   Per-video event/topic markers (called "setlists" historically though they're more like topic chapters here). Many non-OKYUJI videos don't have entries; that's expected.
3. **`aliases.json`** — local file in this repo
   User-editable list of fan nicknames for shows. Same format as the BAND-MAID_live aliases file.

24-hour `localStorage` cache. The **⟲ Refresh** button forces a fresh fetch.

## Layout

- **Sidebar (left)** — category checkboxes with counts; "All" / "None" buttons. When "Self-cover" is checked (or no categories are filtered), member chips appear below to further narrow self-cover videos.
- **Main grid (center)** — card grid showing each show with thumbnail, title, category badge, member/date metadata. Multi-part shows display a "N parts" badge.
- **Detail panel (slides in from right)** — opens when a card is clicked. Shows full metadata, a click-to-open Prime card, part tabs (for multi-part shows), the setlist/topic markers, and a related-videos panel for `previous`/`next` series chains.
- **Search** — searches title, members, song fields, setlist song names, venue, date, aliases, and category simultaneously. Cards highlight when matched; song-match cards get a gold border and a count.

## How videos are grouped

Same logic as BAND-MAID_live, with one addition: the composite group key includes the canonical category. So:

- A multi-part show like "BAND-MAID NEW YEAR PARTY 2026" becomes one show with 2 parts (the two videos share title + date + category).
- "Self-cover - KANAMI" entries are normalized to category "Self-cover" with `members: ["KANAMI"]`. So the sidebar shows one "Self-cover" checkbox instead of one per member.
- Multi-member self-covers like "Self-cover - KANAMI, MISA" become one entry with `members: ["KANAMI", "MISA"]` and surface when either member chip is selected.

## Series via `previous`/`next`

Some categories (especially Stationhead) are sequenced via `previous`/`next` links in setlists.json — each video stands alone but there's a logical order. These are NOT collapsed into a single show entry (they're not multi-part recordings, they're related episodes). Instead, the detail panel shows a "Related" section at the bottom with previous/next links, so once you're inside a Stationhead episode you can navigate to the surrounding ones.

## URL state

All current view is reflected in the query string:
- `?q={query}` — search filter
- `?cat={cat1}|{cat2}` — selected categories (pipe-separated)
- `?mem={m1}|{m2}` — selected member chips
- `?show={showKey}` — open detail for this show
- `?part={index}` — selected part (for multi-part shows)
- `?t={seconds}` — timestamp for the open Prime link

## `aliases.json` format

Same as the BAND-MAID_live repo:

```json
[
  {
    "showKey": "BAND-MAID U.S. TOUR 2022 BEHIND-THE SCENES",
    "aliases": ["US Tour 2022 BTS", "US Tour BTS"]
  }
]
```

## Notes

- Iframe embedding of Prime videos is blocked by Prime's `X-Frame-Options`, same as BAND-MAID_live. The detail panel's video card opens the video in a new tab on Prime instead.
- Member/song fields on self-covers come from prime.json's `Members` and `Song` columns directly — these are well-populated. Searching by song name will surface every self-cover for that song across all members.
