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

- **Miss rate, the number for leaders [data, 2026-10-07]**: missed pings were 2.3% of pings before 12 Aug (25/1,085) and 18.0% after (110/611), about 7.8x or a 681% rise; weekly 21.5, 17.7, 14.8, 12.7%. Quiet four 2.9% to 58.9% (only 56 pings); other twelve 2.1% to 13.9%. Report the total group as rates, not "8x", since the quiet four were picked by outcome and the sample is small. The wait change shows in `pings`: gap after a miss about 92s to about 62s, switching on 12 Aug.
- **Seasonality [data]**: callouts per day fell from 20.0 to 16.9 after 12 Aug and are creeping back, so Priya is partly right about volume. It can't explain a per-ping miss rate, the step on release day, or the quiet four. Still no prior-year data to test it.
- **Tickets undercount the quiet group [data]**: Aunt Dot, Kip and Halloran filed no tickets at all, before or after 4.2, so Vesper and Meteor Mite show as quiet in pings but have zero tickets, while Corporal Ashgrove and Halfmoon have 5 quiet tickets each but only ordinary miss rates. Pings data is the better source for who is affected. Farlight and The Undertow (11 quiet tickets each, all open) are in both lists.
- **Routing arithmetic [code, read not run]**: acceptance is worth about 19 minutes of travel now (about 40 before 4.2), so 4.2 softened the demotion and the weights alone don't explain the quiet four. No decay, so time doesn't help: from score 0 it takes about 7 taken pings in a row to reach neutral, and 60% acceptance to hold steady. Whether `_scores` persists or resets on restart is unknown.
- **Still unproven**: slow accepts caught by the 60s wait vs pushes not delivered. No response times, scores or delivery logs exist in the database or files. Vesper's handler said in her 3 Sep interview that the phone buzzes but Vesper can't reach it in time, which leans slow; one account only. Why the quiet four began missing on 12-14 Aug is open.

- **Routing code, Module 4 read-through [code, read not run]**: `offer.py` `dispatch` calls `record_declined` on anything that isn't "taken", so a miss and a decline are scored the same on purpose. Scores are kept by cover name in an in-memory dict in `history.py`; nothing in the folder adds points except a taken ping (+0.08), and nothing restores, resets or lets a handler adjust a score. A quiet responder is not penalised for silence, since a score only moves when pinged; the way back is to get pinged (needs to be much closer, have rarer skills, or have everyone above decline) and take about 7 in a row, or hold 60% acceptance.
- **What 4.2 changed in scoring [code]**: the weights (proximity 0.45 to 0.60, recent acceptance 0.40 to 0.25) changed how much the record counts in ranking, not the stored score itself, and softened the demotion. The 60s wait is the likelier trigger, since more misses means more -0.12 hits. The +0.08 and -0.12 carry no "was" note, so whether they changed in 4.2 is unconfirmed. Git history in the folder is one commit (6 Oct), so it can't show later edits.
- **Quiet four were not habitual decliners [data]**: before 4.2 Vesper, The Undertow, Meteor Mite and Farlight had 306 pings, 231 taken (75.5%), 66 turned down (21.6%), 9 missed (2.9%). After: 56 pings, 13 taken, 10 turned down (17.9%), 33 missed (58.9%). That is a partial answer to Marcus's 14 Aug question (no distinction in the code; those hit were ordinary accepters), but whether it was intended is still unanswered by Wen Li or anyone else. Not yet compared against other responders' decline rates before 4.2.
- **Gaps in the code [code]**: no delivery check (a missed push looks like a slow answer), no response times or scores logged, no fallback when the list runs out (untaken callouts doubled), no cap on load for top responders, no check for a coverage gap, availability read once per callout, the three phone functions in `offer.py` are empty stubs, and `availability.py` says it writes the record Supply reads but only contains reads.
- **Added to `4-2-investigation-asks.md`**: Wen Li question 5 on how a quiet responder comes back (walkthrough is now 6). Still not added: what problem the 60s cut was meant to solve and how 60 was chosen. The roadmap item "Ping timeout tuning" and the 4.2 notes give no reason; the "wide-geography" reason belongs to the ranking change.

- **Module 5 build [2026-10-09]**: Helen asked for a one-pager and clickable view of what we'd build, not a quiet number change. Saved in `05-super-speed/`: `brief.md` (three changes: a second chance for quiet responders, a miss isn't a decline, handler visibility with reasons and a load/standing list), `one-pager.md`, and `prototype.html`. The brief's sequence is the miss penalty first, a way back second (ease scores toward neutral, the 2019 TODO), handler visibility last. That sizing is my read of the code; Wen Li and Marcus haven't confirmed it. Helen hasn't answered yet.
- **Prototype behaviour is placeholder**: it switches between Meteor Mite with Kip and Vesper with Aunt Dot, has four stages (Today plus the three changes), and a 60s or 90s ping wait with a guessed reach time. Only the 0.60/0.25/0.15 weights are real. Distances, standings, penalties and the "well placed" rule are made up.
- **What the prototype shows**: at Today a longer wait doesn't help a responder ranked last, since they're never asked. At stage 2 a 90s wait helps only if the cause is slow answers, not an undelivered push. There is still no response-time or delivery data.
- **From the 3-4 Sep interviews [wiki]**: Kip's two cards for Meteor Mite and The Gale looked like "two different products"; Mite texted asking if something was broken. Aunt Dot described both vanishing offers and quiet stretches for Vesper, and had not linked them. She asked for a handler alert when a ping comes in (not in the brief). Neither handler's pronouns are stated, and the Aunt Dot transcript carries personal household detail that stays out of our work.
- **Open in the brief**: "What I need" says handler visibility needs no new data, but "Where to start" says it needs score logging that doesn't exist. Fix before sending it on.

- **Module 6 session, 2026-10-09**: built a reusable `review-checklist` skill in `.claude/skills/review-checklist/SKILL.md` (six criteria: problem, customer/user, evidence, value/outcome, solution, risks and dependencies; five-part output; never rewrites a brief unless asked; asks for missing data instead of inventing it). I dropped "Stakeholders and next steps" and "Clarity" on purpose. It is scheduled as `monday-brief-review` (Mondays about 9am, read-only on `05-super-speed/brief.md`); runs only while the app is open.
- **Module 5 brief updated after review [2026-10-09]**: the "What I need" contradiction is fixed (handler visibility is the largest piece and needs score logging). It now has baselines, targets marked "not set" for Helen, a guardrail on untaken callouts and time-to-accept, a definition of "quiet", proposed owners, and the capability-match risk on second chance. Targets, thresholds and owner dates are still unagreed.
- **Correction on "the four" [data]**: the 75% is their acceptance of the pings they were offered (231 of 306), not their share of all pings. The 306 is about 28% of the 1,085 pings before 12 Aug. Confirm with Ravi before quoting a share of load.
- **Another student's brief ("A fair race for Vesper", jesszalecki) reviewed for practice**: its "20-35% more pings" for the other four doesn't match our notes (about 12-15 to 20-23 a week, roughly 50-60%), and its "acceptance 64%" matches none of our weekly figures. Not our data; unverified either way.
