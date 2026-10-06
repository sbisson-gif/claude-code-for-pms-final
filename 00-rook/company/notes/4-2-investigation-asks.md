# 4.2 investigation: asks for Marcus and Wen Li

Draft, not sent. Data is from `rook-database` (29 Jun to 6 Sep 2026) and the `dispatch-routing` code. Edit freely.

## Context in four lines
- Acceptance fell from ~77% to ~54%, recovering to ~73% in the week of 31 Aug. All of the drop is *missed* pings; declines are flat.
- Four responders (Vesper, The Undertow, Meteor Mite, Farlight) went from 75% accepted to ~20%, and from ~28% of all pings to ~9%. Their misses jumped from 1–5% to 50–64% from 12 Aug.
- In the code, a miss costs the same as a decline (-0.12) and a taken ping earns +0.08, with no decay. I think the shorter wait started the misses and the scoring keeps them down. I want to confirm that before I say it to anyone else.
- I'm not proposing a revert. 4.2 was a requested change. I'm trying to understand it.

## For Marcus Oyelaran (engineering manager)

1. **Push logs.** For Vesper, The Undertow, Meteor Mite and Farlight, can you pull delivered and opened timestamps for every ping since 12 Aug? I want to know whether the pings arrived, and how many seconds after delivery they were opened. Pings opened between 60 and 90 seconds would point at the wait. Pings never delivered would point at push.
2. **Devices.** Do the four share a phone model, OS or app version? Did the 4.2 duplicate-push fix change delivery for any of them?
3. **Single-ping chains.** Callouts whose only ping was missed went from 3 (before 4.2) to 21 after. Why does the chain stop there? Is that how the data is logged, or does the offer really stop?
4. **A trial.** Would a short trial of the 90s wait (all responders, or the four) be feasible? Your 14 Aug question about responders who'd been turning jobs down is still open too.
5. **Data after 6 Sep.** Can you or Ravi extend the pings table past 6 Sep so I can see whether the four recovered?

## For Wen Li (staff engineer)

1. **Missed versus declined.** `record_declined` treats both the same, and a miss (-0.12) costs more than a taken ping earns (+0.08). Was that deliberate? Was it reconsidered when the wait went from 90s to 60s?
2. **No decay.** The 2019 TODO in `history.py` asks whether scores should drift back toward neutral. Would that help responders who've stopped being pinged?
3. **Real scores.** Can you pull the current recent-acceptance scores for the quiet four and for a few top responders (The Gale, Nightwell)? Are scores persisted, or held in memory and reset on restart?
4. **Weights.** How much does a floored score matter at 0.25 weight, compared with a minute of travel time? Were 0.60/0.25 tested against the 60s wait together, or separately?
5. **Walkthrough.** Can you take me through `routing.py`, `history.py` and `offer.py`? It's also the start of the written description of how ping decisions are made, which I owe the team.

## Tone
Both are "help me understand" conversations. Lead with the data, ask what they see, and don't present a conclusion.
