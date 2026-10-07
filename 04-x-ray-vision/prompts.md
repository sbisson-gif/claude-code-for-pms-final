# 04 · X-Ray Vision — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. Last session the numbers showed you what happened: a
ping used to wait ninety seconds and now it waits sixty, people
missed pings they used to catch, and missing one counts the same as
turning one down — so four responders stopped hearing from us
altogether.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.

this is interesting. so if a superhero misses a bunch of calls, there score goes down, which then in turn moves them down the list on even receiving the call?

### 2.

Where in the code does it determine the needed response time of 60 seconds?

### 3.

Was this change made to improve an experience? if so what was the intention?

### 4.

now look for what the code doesnt do but should.

### 5.

can you provide me a visual of the experience?

### 6.

Has anything in this code changed recently? Walk me through what's different, and why it would matter to a responder.

### 7.

Based on what I found, the reason some responders are getting no pings at all is ___, because ___.

### 8.

Based on what I found, the reason some responders are getting no pings at all is their recent acceptance scores have dropped and can not recover, because they are unable to respond or missing the response within the new 60 seconds timeframe.

### 9.

did the weighting change in the code after 4.2?

### 10.

fair. Did the weighting change in 4.2? impacting superhero scores?

### 11.

Quick one, did the change to who gets asked first apply to responders who'd already been turning jobs down, or just new ones? Can't tell from the code and don't want to guess on this. Anyone know? Which file has last session's numbers in it? Read that alongside this code. Does it back up what I found in Lab A? And does that answer Marcus's question above?

### 12.

can you answer marcus in one sentence?

### 13.

is it that started missing pings and that resulted in lower scores and less oppertunities?

### 14.

can you provide that detail in the one sentence response as well?

### 15.

how is the code base tracking the score for each of the responders?

### 16.

Find me the part of this code that takes points off somebody when they miss a ping or turn one down. Show it to me and explain it in plain English. Then find me every single thing in this code that puts points back on.

### 17.

What would someone have to be able to do to come back from a low score? Somebody that has been quiet for a month. Walk me through, step by step, exactly what would have to happen for them to start getting work again.

### 18.

according to my findings in the code for someone whos gone quiet, they would need to what?
