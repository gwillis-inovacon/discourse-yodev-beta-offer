# Asset provenance

These images ship **in this repo** rather than as Discourse uploads. That is
deliberate: the previous set was hotlinked from the forum's own CDN and every
one of them was eventually deleted. Discourse garbage-collects uploads that no
*post* references, and an upload referenced only from a theme component's HTML
looks orphaned to that job. Repo assets have no such lifecycle.

**Do not replace these with forum upload URLs.** It works for a while, then
silently 404s — it has already happened twice (see the earlier commit
"replace broken screenshot with Workplace marquee").

## workplace-marquee.png

yoDEV Workplace editor screenshot. Ours. Downscaled to 1200px wide from
`workplace-editor-ai.png` in the yoDEV homepage repo (source is 2514×1592).

## dev-1.jpg, dev-2.jpg, dev-3.jpg

Sample developer avatars for the fictional profile cards, 128×128 (rendered at
44px, so this covers 2×/3× displays).

Fetched 2026-08-17 from Unsplash, carried over from the original homepage
(`src/App.tsx` in `yoDEV-Homepage-NEW-Cursor`, where they were hotlinked):

| file | source URL | imgix id |
|---|---|---|
| `dev-1.jpg` | `https://images.unsplash.com/photo-1507003211169-0a1dd7228f2d` | `3811ff873865ad30bde449218d05b6f3f87c8db1` |
| `dev-2.jpg` | `https://images.unsplash.com/photo-1494790108377-be9c29b29330` | `e69aff7e15221dc0f3fd57f3237ca10e5c1aa151` |
| `dev-3.jpg` | `https://images.unsplash.com/photo-1500648767791-00dcc994a43e` | `48e7e70cd7951a5b61079dfd755528265fc7cec6` |

### ⚠️ Open licence question — not resolved

The Unsplash Licence grants the **copyright** side (free commercial use, no
attribution required). It does **not** grant personality rights, and Unsplash's
own guidance says permission is needed where a photo implies a person endorses
a product or service.

These are photographs of real, identifiable people used to depict **fictional
developers** with names, hourly rates, review counts and availability, on a
surface selling a commercial marketplace. That is the situation the carve-out
describes. Risk is low in practice but not zero, and Greg is investigating
(2026-08-17).

**Tracing the origin:** the `photo-<timestamp>-<hash>` segment is an imgix file
path, not Unsplash's API id, so there is no ID lookup — `unsplash.com/photos/…`
404s for it and Unsplash blocks scripted requests. Use **reverse image search**
(Google Lens or TinEye) on the file to reach the photo page, photographer and
licence terms.

**If it doesn't clear:** `FeaturePreviews.tsx` in the same homepage repo renders
these cards as initials monograms on coloured backgrounds (`MS` / `#6366f1`,
`VT` / `#ec4899`, …). That is a drop-in replacement needing no images at all.
