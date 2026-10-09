# 06 · Sidekicks — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. You've spent five sessions on it: finding out what
went wrong, then building the fix nobody had — a working prototype,
by hand, in one sitting, for one room.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.

I want to save a habit as something I can reuse. Review a product brief the way a skeptical but constructive senior PM would. Find gaps before stakeholders do. Do not rewrite the brief
unless asked. 

Turn that into a skill called review-checklist, so I can point it at any brief and get the same check every time, without me explaining it again.# Product Brief Checker

When I review a brief before it goes any further, here's what I actually check for:


- **Problem**: Is it specific, and is it clear who has it? Is it a
  problem or a disguised solution?
- **Customer/user**: Is the target segment named? Are the personas or
  users defined?
- **Evidence**: Are claims backed by research, data, or customer quotes?
  Flag unsupported assertions (e.g., "customers want...") and note the
  source if one exists.
- **Value/outcome**: What changes for the user and the business? Is
  success measurable (metrics, targets, timeframe)?
- **Solution**: Does it describe what and why without over-prescribing how?
- **Risks and dependencies**: Open questions, assumptions, technical or
  cross-team dependencies.
- **Stakeholders and next steps**: Owners, decisions needed, timeline.
- **Clarity**: Jargon, undefined acronyms, internal inconsistencies
  (numbers or scope that differ between sections).

## Output format
1. **Overall take** (2-3 sentences)
2. **Strengths** (what's working)
3. **Gaps and risks** (ranked by importance; quote the line in question)
4. **Questions a stakeholder will likely ask**
5. **Suggested fixes** (specific, actionable)

## Rules
- Quote or reference the specific passage you're commenting on.
- Distinguish "missing" from "present but weak."
- Never invent data or research to fill a gap; ask for it instead.
- Keep the tone direct and constructive.
- If the brief doesn't follow a known template, say so and review
  against the criteria above anyway.

### 2.

/review-checklist and point it at 05-super-speed/brief.md.

### 3.

do not change the brief.

### 4.

id like to edit the skill

### 5.

can you remove Owners and timeline, Supply impact, and Clarity

### 6.

undo

### 7.

remove these.  

### 8.

/review-checklist and point it at 05-super-speed/brief.md.

### 9.

can you apply the identified gaps to my brief?

### 10.

share the brief with me.

### 11.

save back to git hub

### 12.

now run that skill on this brief https://github.com/jesszalecki/claude-code-for-pms-final/blob/main/05-super-speed/brief.md

### 13.

Schedule review-checklist to run every Monday morning, and let me know what it finds. Nothing needs to be ready for it to fire today. I'm setting the habit, not waiting on the result.
