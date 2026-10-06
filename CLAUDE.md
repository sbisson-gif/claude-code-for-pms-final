# Rook Industries — course working file

## Session scope — Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Read and write only within this directory.
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.ccpm/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); and rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it).
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

Sources: Priya's handoff (`00-rook/company/notes/handoff-from-priya.docx`, 21 Aug 2026) and the rook-wiki connector (read 2026-10-06). Tagged **[handoff]**, **[wiki]**, **[data]** (`rook-database`, pulled 2026-10-06) or **[inference]**. Data covers 29 Jun to 6 Sep only, so "September" is one week. Today is 2026-10-06, about eight weeks after 4.2 shipped.

### Me and my role
New PM for **Rook Dispatch**, the flagship. I report to the Director of Product. Predecessor Priya Raghunathan was the only Dispatch PM for 14 months, left 21 Aug with no overlap, and admits she made calls faster than she checked them. Low-scrutiny areas are the likeliest home of bad old decisions; my fresh-eyes window is short.

### The company and products [wiki]
Rook sells coordination software to independent masked responders and their handlers; subscription, priced per active responder. Monthly release train, 4.x numbering. Two products:
- **Dispatch** (mine). Handlers use the web **console** (stable; no dark mode yet). Responders use the **phone app** (stable since 4.1). **Routing** ranks available responders per callout, pings the top, and walks down the list on decline or miss. It sets the *order* only. Routing config ships with the release; handlers can't adjust it. Code: `00-rook/code/dispatch-routing/` (`history.py`, `availability.py`).
- **Supply**: gear requisitions, maintenance, failure reports. It reads the **Responder Availability Record**, which Dispatch writes. Changes to how Dispatch updates that record flow into Supply's maintenance scheduling with no Supply-side change.
- **Hard constraint**: responder cover identities are never stored, and no mapping to legal identity exists. Read Security Policy 4.1 before touching responder records. Never try to work out who anyone is.

### People [wiki directory, updated 2 Sep]
- **Helen Achebe**, Director of Product (my boss; owns roadmap and commitments). Gives room [handoff].
- **Marcus Oyelaran**, Engineering Manager, Site Aleph. Straight talker; first stop [handoff]. Backup owner of routing.
- **Wen Li**, Staff Engineer, Berlin. Built routing; the only real source on how ranking works. Was away 14–24 Aug.
- **Nadia Hoffmann**, Support Lead, Berlin. Owns the tickets. Worth a standing 15 minutes.
- **Ravi Menon**, Data Analyst, Singapore. Owns the real weekly "how often responders take pings" numbers. The handoff points to the EM for numbers, but Ravi is the source.
- **Sofia Marino**, Product Designer. Owns console and phone app; ran the September handler interviews.

### Vocabulary [wiki glossary]
- **Callout**: request for a responder to attend an incident. **Ping**: a callout offered to one responder's phone. Outcomes: **taken**, **turned down**, **missed** (no answer before the **ping wait**; recorded separately, but both move it on).
- **Ping wait**: how long a ping sits before counting as missed. Same for everyone, set in the release (what Priya called "ping timeout").
- **Acceptance rate**: headline metric. Share of pings *taken* rather than turned down or missed. Reported weekly, in aggregate. **Time-to-accept**: median seconds, ping sent to taken. **Coverage gap**: no available responder had the required capability tags (nobody *could* go, not nobody *would*).
- **Routing priority**: the score that orders responders. Inputs: proximity (travel-time estimate since 4.1), current availability, capability match, **recent acceptance history**. Turning down or missing a ping lowers that last component, so lowers later rank.
- **Capability tags**: flight, structural-entry, hazmat-tolerant, cold-weather, aquatic, crowd-management, de-escalation.
- **Mutual aid / shared cover**: responders covering for each other across the city. Not supported; Q4 exploring.

### Release history [wiki]
4.0 (7 Apr): console nav, profile redesign, routing-override audit log. 4.1 (16 Jun): travel-time proximity, bulk callout, push reliability. **4.2 (12 Aug)**: proximity weighted up vs recent acceptance history; **ping wait cut 90 to 60s**; console filters persist; three defect fixes.

### Where things stand
**The 4.2 problem.** Fewer pings taken and more handler complaints since release.
- **Priya's hypothesis [handoff]**: mostly seasonal (August is always soft), recovering in September. Unverified. Her steer: don't frame it as "revert 4.2"; the change was asked for by wide-geography responders (nearby people unoffered while the system reached ~40 min away for a better record).
- **Support evidence [wiki, 4.2 comments]**: tickets ~3x normal from 18 Aug, still elevated 26 Aug. Roughly two thirds "my phone never goes off", one third "buzzed, but gone before I could answer". Support lead says the 60s cut explains the second theme but not the first.
- **Interviews [wiki, 2–5 Sep]**: handlers describe lopsided load, with some responders silent for weeks and others constantly busy and losing the race on pings (Kip: two responders, same city, same week, "all the way to one side or the other"). Ambrose says slower responses used to still land the job; lately they don't.
- **Weekly numbers [data]**: acceptance 75–78% for six weeks before 4.2, then 54% (wk of 10 Aug), 66%, 67%, 73% (wk of 31 Aug). Turned-down pings stayed flat; the whole drop is *missed* pings (2–6/wk before; 38, 28, 24, 21 after). Partial recovery, still below baseline. No prior-year data, so seasonality can't be tested.
- **By responder [data]**: four responders (Vesper, The Undertow, Meteor Mite, Farlight) took 75% of pings before 4.2, but in 12–31 Aug took 13 of 53 with 30 missed, and got 0–1 pings each in the week of 31 Aug. Nightwell, The Gale, Stormwrack, Captain Vantage went from ~12–15 to ~20–23 pings/wk. Not geographic: Meteor Mite and The Gale share Eastgate. No response-time or score data, so *why* those four is unknown.
- **Untaken callouts [data]**: 5.5% before (48/879) vs 11.1% after (49/440). Callouts whose only ping was missed went from 3 to 21. Unexplained: why the chain stops after one ping. Ask the EM.
- **Tickets [data]**: ~6/wk before; 20, 27, 32, 25 in the four weeks after (~83 of 107 post-release tickets still open on 7 Sep). By subject, 14 "phone never goes off" and 13 "gone before answered" (crude match). Filter persistence is not just noise: several tickets about silent resets and shared computers.
- **Routing code [code, read not run]**: `config.py`: a taken ping adds 0.08 to a responder's recent-acceptance score, a turned-down *or missed* ping subtracts 0.12 (break-even is 60% acceptance), floor 0, no decay (2019 TODO in `history.py` never resolved). Weights now 0.60 proximity / 0.25 acceptance / 0.15 capability (were 0.45/0.40/0.15); offer wait 60s (was 90). `_scores` is an in-memory dict; production persistence unknown. The offer walks down the ranked list, so low-ranked responders are rarely reached.
- **Wait vs misses [data]**: the quiet four's miss rate went from 1–5% to 50–64% (day and night alike), starting 12–14 Aug, *before* their ping volume fell (43, 16, 6, 3 pings/wk). Everyone else went from 2% to ~14%. Declines are always answered in 6–40s; misses run the full wait (~92s before, ~62s after). Accept times are unmeasurable (no next ping). Misses come first, demotion second. Not yet separable: slow accepts caught by 60s vs pushes not delivered (4.2 also changed duplicate-push handling). Decisive checks: push delivered/opened logs, device types, a 90s trial.
- **Open engineering question [wiki]**: on 14 Aug the EM asked whether the proximity change was meant to apply to responders who'd been turning jobs down. The config doesn't distinguish. The staff engineer said she'd look on return; no answer is recorded.
- **[inference] to test, not conclude**: seasonality can't explain a split between quiet and overloaded responders. A feedback loop is plausible: shorter wait gives more misses, misses lower rank, lower rank gives fewer pings. The aggregate acceptance rate would hide this. Weights, wait cut and seasonality are confounded. The by-responder data fits this loop but doesn't prove it. Next: get Wen Li on how misses feed rank, and Ravi/EM for data after 6 Sep and for response times.
- **[inference]** Uneven load could also shift Supply's maintenance windows via the availability record. Check with Supply.

**Roadmap [wiki, last reviewed 30 Jun, stale].** Q3 items committed for 4.2: who-gets-pinged change and ping timeout tuning (both shipped), and **Availability Confidence** (confidence score beside stated availability), which is *not* in the 4.2 notes, so it was squeezed out. The handoff says "a couple" were squeezed; I can't identify the second. Requisition approval chains (Supply) is committed for 4.3. Handler phone app and shared cover are Q4 "exploring". Deciding what is still a Q3 commitment is an open conversation with Helen.

**Other open items.** Filter persistence will produce tickets: cosmetic, low priority [handoff]. Ambrose says it has reset to default twice and wants a warning [wiki]. Other handler asks: dark mode (Kip, repeatedly), bigger status text, per-responder alert sounds. **No written description of how ping decisions are made. I should write it, with Wen Li** [handoff].

**Wiki hygiene.** Two product briefs look stale: Routing Override Audit Log (5 Mar, "ready to build") matches an item already in 4.0, and Bulk Callout (12 Jan, "nobody's picked it up") shipped in 4.1. Verify before relying on either.

### Session notes, 2026-10-06
- The problem is two-tiered: acceptance fell ~8–10 points for nearly everyone (consistent with the 60s wait), and four responders (Vesper, The Undertow, Meteor Mite, Farlight) collapsed to ~20% acceptance and almost no pings. Priya's seasonal story doesn't explain the second part.
- The suspected mechanism is in `config.py`/`history.py`: a miss costs the same as a decline (-0.12), a taken ping earns only +0.08, and scores never decay. Read from the code, not confirmed with Wen Li or real scores.
- Still open: whether the cause is slow accepts caught by the 60s wait or pushes not delivered. Push delivered/opened logs for the quiet four settle it. Data stops at 6 Sep.
- Drafted, not sent: `00-rook/company/notes/4-2-investigation-asks.md` (questions for Marcus and Wen Li). Interview Wen Li first.
- Don't frame any of this as "revert 4.2". Still to do: Q3 conversation with Helen, and writing up how ping decisions are made.

### How to help me
- Separate what's sourced (handoff, wiki, data) from inference. Priya's note is one person's view.
- Don't invent names, numbers or metric definitions; flag gaps. Open gaps: data after 6 Sep, response times, actual routing weights and how misses affect rank, the second squeezed-out item.
- Numbers come from Ravi Menon, the engineering manager, or the `rook-database` connector (`.mcp.json`).

- **Interviews vs tickets [wiki, data]**: the four handler interviews (Aunt Dot/Vesper, Ambrose/Captain Vantage, Halloran/Sgt. Bulwark, Kip/Meteor Mite and The Gale) and the 147 `support_tickets` (11 regular filers, one responder each, plus Ambrose) barely overlap, so they work as two independent samples that describe the same two-tier pattern. Dot, Kip and Halloran filed no tickets.
- **Responder lists disagree**: tickets name Farlight, The Undertow, Corporal Ashgrove and Halfmoon as quiet (30 tickets, all still open, filed by Okafor, Pruitt, Fischer, Demir; about 85% of their post-4.2 tickets open). The earlier data pull named Vesper, The Undertow, Meteor Mite, Farlight. Only The Undertow and Farlight are in both. Confirm with Ravi and Nadia before quoting "the quiet four".
- **Ticket timing [data]**: tickets run about 6 a week to 20, 27, 32, 25. "Gone before I could answer" tickets (15) spike first (week of 10 Aug) and fade; "phone quiet" tickets (30) start a week later and keep going. Last week in the table is one day only. Push-delivery tickets (5) all predate 4.2. Supply tickets were steady, 13 before and 13 after.
- **Before 4.2 was already lopsided [data]**: four responders took 75% of pings, so 4.2 flipped the skew. Not a new unevenness from nothing. Overload has no ticket equivalent; it shows only in interviews (Kip) and ping data.
- **Weak or unsupported**: the costume or suit-up idea rests on Ambrose's anecdote (also his ticket 3043) and Halloran's secondhand remark; no response-time data. Halloran said maintenance scheduling had improved, but tickets 3054 and 3109 show maintenance booked on marathon day.
- **Next three actions**: (1) ask Ravi for per-responder data after 6 Sep for all eight named responders, Marcus for push logs, Wen Li on how misses affect rank; (2) have Nadia send a short honest holding reply to the four waiting handlers, agreed with Helen; (3) take the two-tier story to Helen, with per-responder monitoring as the success measure and a Q3 commitments decision.
