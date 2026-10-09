# Giving quiet responders a way back — one-pager (draft)

For Helen · 2026-10-09 · Draft for discussion, not a commitment. Clickable mock: `prototype.html` (same folder, open in a browser).

## The problem, from the person it happens to

- **Responder who went quiet** (e.g. Vesper): missed a few pings in mid-August, dropped down the list, stopped being offered work. Nothing in the app says so. The phone is silent and there is no way back short of being pinged. [data/code]
- **Handler (e.g. Kip)**: sees some responders always busy and others silent for weeks, and can't tell why or do anything about it. [wiki interviews]

Why it sticks [code, read not run]: a missed ping costs the same as a decline (-0.12), a taken ping earns +0.08, and the score never decays. The score only moves when someone is pinged, so silence doesn't heal it. The way back is about 7 taken pings in a row from the floor, and you have to be pinged to get them.

## What we'd build instead

Three things, each visible to a person, not a setting. All three are proposals; the mechanism choices need Wen Li and Marcus.

1. **A second chance, not a silent demotion (responder).** A responder who has gone quiet is offered the next callout they are well placed for, and told so in the app: "You've been quiet. This one's close to you." A taken ping from this slot counts for more than a normal one. *Open: what "well placed" and "counts for more" mean in numbers. I haven't picked any.*
2. **A miss is not a decline (both).** Score misses separately from turn-downs, and don't penalise a miss until we know the push arrived. *Depends on delivery logging, which doesn't exist today [code gap].* Until then, an interim option is a smaller miss penalty.
3. **Handlers can see why (handler).** On each callout, show the order we'll ask in and one plain reason per responder ("closest", "quiet for 3 weeks, second chance"). A weekly "who's carrying the load / who's gone quiet" list on the console, so Kip sees the lopsidedness we found in the data rather than discovering it.

What the mock shows: Kip's callout view with reasons and a quiet list; a responder's phone with the second-chance ping and a standing card; a toggle for today vs proposed.

## How we'd know it worked

Per-responder, not aggregate (the aggregate hid this): pings per responder per week for the previously quiet group; their miss rate; share of responders getting at least one ping a week; untaken callouts (5.5% before 4.2, 11.1% after). Acceptance rate stays a secondary check.

## Not claiming

- The cause is slow accepts caught by the 60s wait versus pushes not delivered: **unproven**. The proposal doesn't depend on it, except item 2.
- The "quiet four" list is disputed between data and tickets. Confirm with Ravi and Nadia.
- Not "revert 4.2". Whether to restore 90s is a separate question for Wen Li and the data after 6 Sep.
- Whether `_scores` persists in production is unknown, which changes how the fix is shipped.
- Mock numbers and names are illustrative, not data.

## Decisions for you

1. Are all three in scope, or do we start with 1 and 3 (no new data dependency)?
2. Q3 commitment: does this displace anything? (Availability Confidence is still unshipped.)
3. OK for me to take the mechanism to Wen Li and Marcus before anything is built? The effects on Supply via the availability record also need a check.
