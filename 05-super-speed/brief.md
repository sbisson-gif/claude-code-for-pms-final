# Product brief: a way back for quiet responders

Draft, 2026-10-09. For discussion with Helen, Wen Li and Marcus. Nothing here is committed.

## What's going wrong

Since 4.2, a handful of responders have basically stopped getting callouts. Nobody tells them it's happening, and handlers like Kip can see the lopsidedness but can't see why or do anything about it. It isn't new, though: four responders took most of the pings before 4.2, and the release only changed who those people were.

- They missed a few pings, and the system treated each miss like a "no thanks".
- Their ranking dropped, which meant fewer pings, which meant no chance to earn it back.
- Nothing in the code ever lets someone recover on their own.

## What we'd build

Three changes. You'd notice each one as a person, not as a setting.

- **A second chance.** When a callout is close to a responder who has gone quiet, offer it to them, and have the app say so: "You've been quiet. This one's near you." If they take it, it counts for more than a normal accept, so they can climb back.
- **A miss isn't the same as a no.** Someone who declines made a choice. Someone who didn't answer might have a dead phone or a push that never arrived. Stop penalising misses until we can tell which it was.
- **Handlers can see what's going on.** On each callout, a one-line reason next to each name ("closest", "second chance, quiet for weeks"). On the console, a simple list of who's carrying the load and who's gone quiet.

## Where to start

This is my read of the code, not a sizing from engineering, so Marcus and Wen Li should confirm it. The quick fix is making a miss cost less than a decline. On its own it stops new damage but doesn't help anyone already stuck near the bottom, so I'd pair it with the simplest way back.

- **First, a miss isn't a no.** The smallest change, probably hours. Add a separate, smaller penalty for misses. It needs no app change and no new data. The version that waits for push delivery logs is not the quick one.
- **Second, a way back.** Medium effort. The simplest version is to let scores ease back toward neutral over time (the 2019 TODO), with no new screens. The "app tells you" message needs a phone app change, which is separate work.
- **Third, handler visibility.** The largest piece. It needs console design with Sofia, and the code doesn't log scores or reasons today, so there's nothing to show yet. Start the design now in parallel, and ship it after the other two.

Before any of it ships, get the per-responder weekly measure running, so we can show it worked rather than quietly changing a number. Also check with Marcus whether scores persist in production.

## How we'd know it's working

We wouldn't use the headline acceptance rate, which averaged the problem away. We'd look at each responder instead.

- Are previously quiet responders getting pinged again each week?
- Is their miss rate coming down?
- Are fewer callouts going untaken? It went from about 5.5% to 11% after 4.2.

## What we don't know yet

The biggest gap is why the quiet responders started missing. Either they're slow to answer and the 60-second wait catches them, or pushes aren't reaching their phones. Push delivery logs would settle it. The "miss isn't a no" change depends on the answer; the other two don't.

- **Thresholds:** what "quiet" and "close enough" mean in numbers. I haven't picked any. That's for Wen Li.
- **Persistence:** whether scores survive a restart in production. It changes how we ship this.
- **Who's affected:** the ping data and the tickets name different people. Ravi and Nadia need to confirm.
- **Supply:** whether they feel it, through the availability record.

## What this is not

This isn't a revert of 4.2. The change was asked for by wide-geography responders, and that reason still holds. Whether to restore the 90-second wait is a separate question, for after we see data past 6 September.

## What I need

A few conversations before anything gets built. The clickable mock (`prototype.html`) shows all three changes from Kip's and Vesper's side and can anchor them.

- **Helen:** scope (all three, or start with the second chance and handler visibility, which need no new data), and whether it displaces anything in Q3.
- **Wen Li and Marcus:** how a miss feeds into rank today, and what a sensible second-chance rule looks like.
- **Ravi:** ping data for every responder since 6 September.
