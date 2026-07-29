# Obsidian + Claude Code: A Student's Second Brain

straightforward indepth guide to offloading your brain to ai if you're a chud like me

## Quick definitions

- **Obsidian:** a free note-taking app where your notes are plain text files in a folder you own, all linked together. (free) [obsidian.md](https://obsidian.md/download)
- **Claude Code:** an AI agent that reads and writes the files in a folder. (~$20/mo, Claude Pro) [claude.com/product/claude-code](https://claude.com/product/claude-code)
- **Together:** point Claude Code at your Obsidian folder and you get a second brain, notes an AI keeps organized and an assistant that already knows your goals instead of one you re-brief every session.

Install both, make your vault its own folder, open Claude Code from that folder. That's the whole technical setup. Read [The problem](#the-problem) for the why, or jump straight to [Scaffolding from zero](#scaffolding-from-zero) to actually set it up.

---

## The problem

Before this, my actual situation looked like:

- An internship, a student org leadership role, classes, and a FAANG-prep grind running at once, re-explaining all of it to an AI chat every single session
- A backlog of notes scattered across Google Docs, no structure, no way to tell if I'd already learned something
- Goals and priorities that only lived in my head, so "what should I focus on" answers from an AI were generic, not grounded in what I'd actually committed to
- LeetCode practice that felt like a chore, no visible progress, no reason to protect the streak
- Daily work and learnings at my internship that either went nowhere or sat in a notes app no agent could read

The vault fixes all of it.

---

## Index

- [The solution](#the-solution): pairing Obsidian with Claude Code fixes all of it
- [Scaffolding from zero](#scaffolding-from-zero): let Claude Code interview you, then migrate what you already have
- [My setup](#my-setup): a reference folder structure, copy what's useful
- [How I actually use it](#how-i-actually-use-it): the orchestrator, the skills/commands bench, and the agents behind it
- [Syncing across devices](#syncing-across-devices): Google Drive for desktops, what your phone actually gets
- [Notes](#notes): what mattered more than the folders themselves

**In a hurry?** [Quick definitions](#quick-definitions) plus [The solution](#the-solution) is the whole pitch in about two minutes. Want the in-depth version? Read straight through, that's what the rest of this is for.

---

## The solution

Pair Obsidian with Claude Code. Claude Code already reads your whole working folder as context, it just has nowhere durable to keep what it learns about you. Obsidian is that place:

- **Notes are Markdown.** Same format Claude Code already reads and writes.
- **The vault is just a folder.** Point Claude Code at it, full context, no uploads, no separate memory system to babysit.
- **`[[wiki-links]]` connect notes.** Easy for an agent to create and follow, easy for you to browse in the graph view.
- **It's agent-agnostic.** The vault outlives whatever tool you're using it with today.

**Why this matters as a student:** constant context-switching (classes, internship, clubs, job search) means re-explaining your situation to an AI every session is wasted time. With a vault, the agent already knows your goals and active threads.

### What it fixes

Straight across from the problems above:

- No more re-explaining my whole life every session, the agent reads my goals and live threads from the vault
- Scattered Google Docs turned into linked, filed notes I can actually search and reuse
- Priorities live in the vault, so "what should I focus on" is grounded in what I committed to, not generic
- LeetCode has a visible streak and a reason to protect it
- Daily work and learnings get filed somewhere the agent can read, instead of dying in a notes app

---

## Scaffolding from zero

Don't hand-design the folder structure up front. Let Claude Code interview you instead, ask about your goals and commitments, then draft a structure and get your approval before writing anything.

Starter prompt:

> I want to use Obsidian to help you and me better manage context. The vault is at [folder path]. Ask me about my goals, responsibilities, and how I want to work with you, one question at a time. Once we're aligned, propose a folder structure and starter files. Wait for my approval before creating anything.

The one file that actually matters is **`CLAUDE.md`** at the vault root, read at the start of every session. It should define:

- who you are and what you're working on (link out to a profile/goals note, don't cram it in-line)
- the vault's folder structure, so the agent knows where things go
- writing conventions (naming, linking, tone)
- any standing behaviors you want automated (auto-commit, daily briefs, etc)

### Already sitting on a pile of notes?

- Chances are you're not starting from a blank vault. Mine was a pile of Google Docs with no real structure.
- Connect whatever MCP fits where your notes already live (Google Drive, Notion, Dropbox), point Claude Code at the docs, it pulls them in as Markdown and reshapes them into your structure. Mine was Google Drive.
- No MCP available or it doesn't cooperate? Just export your data (most tools support a plain export) and drop the files into the vault folder directly, Claude Code can still read and reshape them from there.
- This is the main advantage of running Claude Code with MCPs wired in: a multi-day migration chore becomes one conversation.

---

## My setup

Everything above this section is all you actually need to have a working vault. What's below is just a reference, my own folder structure, so you have one real example to copy from instead of starting blank.

```
vault/
├── CLAUDE.md              # entry point, read every session
├── HOME.md                 # daily cockpit / quick links
├── _meta/                  # identity + goals the agent references constantly
├── memory/                 # Claude Code's persistent cross-session memory
├── work/                    # internship/job: daily logs, knowledge base, people, projects
├── career/                  # FAANG prep, LeetCode tracker, resume, LinkedIn drafts
├── org/                     # club or student org: projects, people, knowledge
├── personal/                 # finances, fitness, personal projects
└── .claude/
    ├── commands/              # explicit-only shortcuts: cmt, yur
    └── skills/                # richer capabilities, some auto-trigger
                                # mirrored in _agents/skills/<category> for humans
```

(the trailing comments describe what's inside each folder, not extra root items)

- **Knowledge notes are separate from daily logs.** Logs are a stream of what happened. Knowledge notes are distilled, linked, filed into topic subfolders.
- **`memory/` is not a task list.** It holds durable facts, not in-progress state.
- **Skills and commands are organized twice on purpose.** Flat in `.claude/` for Claude Code, categorized in `_agents/<type>/<category>/` for humans browsing the repo.

---

## How I actually use it

Three layers: one thing that runs without me asking, a bench of things I ask for by name or that fire on their own mid-conversation, and a set of agents that go off and do a whole job alone.

### The orchestrator

The single highest-leverage piece of the three.

**What it is:** my own agent's dispatcher, it wakes up on its own and deploys the other agents below to do the work. Under the hood that's a scheduled cron job (`launchd`, macOS's built-in scheduler). I never invoke it, it just fires once a day whether or not I've opened Claude Code.

**Does it only run when I trigger something myself?** No, that's the point, every skill below only runs when I ask. The orchestrator is the exception, and it's also one half of a loop: closing out my day (`eod`, under Skills below) hands it a priority to check on, so it's not just running on a timer, it's following up on what I flagged the night before.

**What it does, in order, every run:**
1. **EOD handoff check.** If last night's close-out left a handoff behind, check whether that flagged priority actually got done. If it did, the handoff gets cleared. If it didn't, it carries into today's digest instead of quietly vanishing.
2. **Staleness check** against each tracker file's own self-declared due dates. Mechanical fixes (a due date that just needs bumping) get patched directly. Anything ambiguous gets flagged for me instead of guessed.
3. **Weekly `lint` sweep**, checks whether the vault-audit agent already ran in the last 7 days before calling it again, so it never duplicates work.
4. **Live job vetting**, calls the job-scout agent with one rule added on top of its own: never trust a search snippet, fetch the real posting page and confirm date, location, and eligibility before anything lands on my list.

**What it touches:** the eod handoff file (reads then clears it), tracker files (patches due dates directly), the vault-audit and job-scout agents (by invoking them), and one recommendations file that gets overwritten each run, never appended, so it's always current state, not a growing log. It commits what it fixed mechanically. **It never pushes**, that stays mine to review and send.

**Benefits:**
- Follows up on last night's priority without me having to remember to ask
- Catches staleness before I notice it myself, no manual check-ins needed
- Never duplicates a `lint` sweep, checks recent run history first
- Filters out dead or fake job listings by fetching the real page, not trusting a search snippet
- Keeps one current recommendations file instead of a growing pile of one-off reports
- Never pushes on its own, nothing lands remotely without my review

### Skills

Most of what I run I never type. It fires on its own the moment it's relevant: I say "log this" and it files into the right folder without me picking one, I say "I don't get it" and the tutor re-explains the last thing plainer. Those are Skills, and the auto-firing ones are the ones I lean on hardest.

The heavier stuff I still want to pull the trigger on myself, the morning brief, a LeetCode session, closing out the day, are Skills too, just set to explicit-only so they don't fire on a stray mention. Only the two dumbest, most deterministic jobs, commit and push, stayed plain Commands.

The line I actually split on: if it should be able to fire without me, or it grew past a single prompt block, it's a Skill. If it's a one-shot template I'll always trigger by hand, it's a Command.

#### The bench

| Name | Type | Trigger | Does |
|---|---|---|---|
| `daily-full` | Skill | explicit only | morning brief, synthesized chief of staff style |
| `log` | Skill | auto (log this, recapping something that happened) | files anything into the vault without me deciding where |
| `lint` | Skill | auto (mentions of staleness or gaps) plus biweekly | audits the vault for drift |
| `lc-coach` | Skill | explicit only | LeetCode practice, gamified |
| `tutor` | Skill | auto (I do not get it, explain that again) | re-explains the last thing, plainer |
| `quiz` | Skill | explicit only | active recall quiz mode for school |
| `eod` | Skill | explicit only | closes the day |
| `cmt` | Command | explicit only | stage and commit |
| `yur` | Command | explicit only | commit and push |

#### Skills, auto-trigger

These fire on their own when the moment matches, no slash typing required.

**`log`**: feeds anything, a learning, a recap, a reflection, straight into the vault and files it without me deciding where it goes. My most-used skill by a wide margin.

```
# log (Skill)
description: Use whenever the user shares a learning, recap, activity update,
or reflection, and route it into the vault without asking where it goes.

Classify the input, then route it.

| Type               | Destination                        |
|---------------------|--------------------------------------|
| Technical learning  | Knowledge note, right subfolder      |
| Daily activity      | Today's log, work section            |
| Personal            | Today's log, personal section        |
| Mix                 | Split and file each part             |

Extend an existing note if one fits, otherwise create a new one with a source
line and a related-notes link. Commit immediately after writing.
```

**`lint`**: runs every two weeks and audits the vault so I don't have to notice what's gone stale on my own, and can also fire early if I mention a gap directly.

```
# lint (Skill)
description: Use when the user asks to audit the vault, mentions something
feels stale or disorganized, or on the biweekly scheduled run.

Check for:
- knowledge gaps (concepts mentioned, never given a note)
- people gaps (names mentioned, no page yet)
- stale items (todo-style folders untouched 14+ days)
- empty folders that should have content by now
- stale paths (CLAUDE.md vs real folder structure)
- pattern topics (mentioned 3+ separate days, deserves a note)
- broken [[links]]
Regenerate the knowledge index. Report as a short itemized list, then ask
what to create.
```

**`tutor`**: re-explains something already covered, at the plainest level possible, for when an explanation landed too technical or too fast.

```
# tutor (Skill)
description: Use when the user signals an explanation did not land and wants
it re-explained in plain language.

Re-explain the most recent substantive explanation (or a named topic/slice),
plain language first, define every term. Use analogies grounded in something
real, never a made-up hypothetical. Split into short labeled sections.
Actually re-derive it, don't repeat the same wording. Conversation-only
unless separately told to log it.
```

#### Skills, explicit only

Deliberate enough that I don't want them firing on a stray mention, but complex enough to outgrow a plain command.

**`daily-full`**: my most-used skill overall. Every morning it acts like a secretary: instead of raw data, it hands back one synthesized brief of what actually matters today.

- **MCP tooling used:** Slack, Gmail, GitHub, Google Calendar, plus a live web search for open roles
- **What makes it different from a dashboard:** day-aware filtering (weekdays get full context, weekends filter to personal/career only), sections only appear if there's real content, nothing repeats across sections
- **Customization:** the tone of the closing affirmation and the shape of the output are fully mine, rewrite both for your own voice and goals

Trimmed, genericized version of the actual skill:

```
# daily-full (Skill)
description: Use only when the user explicitly asks for their daily brief or
invokes /daily-full by name. A general question about their day or schedule
is not by itself a request for the brief.

Runs the full daily brief. Operate in deep work mode, don't just report data,
synthesize it. Read like a chief of staff who already did the thinking.

## Pull sources in parallel
- Slack: full threads, flag only if genuinely unresolved
- Gmail: work and recruiting related only, skip anything promotional
- GitHub: flag only if action is needed
- Google Calendar: next 7 days
- Personal tracker files: streak, priorities, deadlines within 30 days
- Web search: verified open roles at target companies

## Apply day-aware filtering
- Weekdays: full context, filter for highest leverage today
- Weekend: personal and career only, unless truly urgent
- Second weekend day: add a next-workday prep section

## Output format
No dashboard tables. Write like a briefing. Skip empty sections.

MAIN GOALS / IDENTITY (fixed, shown verbatim)
START HERE (today's single highest-priority focus)
NON-NEGOTIABLES (2-3 must-happen items)
MOVE THE NEEDLE (2-4 concrete actions)
MESSAGES (only if a reply is genuinely needed)
CRUCIAL REMINDERS (deadlines inside 7 days)
OPEN ROLES (verified listings worth applying to)

## Affirmation
3-4 sentences, tone tuned to what actually works for you. Talk directly to
the reader. Real numbers as fuel, not the point. No generic filler.
```

**`lc-coach`**: wraps LeetCode practice in a status brief, a gamified XP/streak layer, and a closing affirmation, so it's a habit instead of a chore.

```
# lc-coach (Skill)
description: Use only when the user explicitly starts a LeetCode practice
session or invokes /lc-coach by name.

Opening brief: streak, weekly count, progress through a fixed problem set,
today's assigned problem, an open "free play" lane for anything else.

Two modes:
- Ranked match: one per day, full formal interview protocol, the only mode
  that flips a problem to "solid"
- Free play: unlimited, casual, lightly logged

Gamification: XP per solve, bonuses for clean/harder solves, a persistent
rank ladder, a visual progress bar.

Tone: nice and supportive always, regardless of session outcome. Standards
don't soften, only the delivery does, the one deliberate carve-out from
everything else's default directness.
```

**`quiz`**: for school specifically, once the semester's in session. Active-recall quiz mode, one question at a time, grades honestly, tracks misses, closes with a score summary.

```
# quiz (Skill)
description: Use only when the user explicitly asks to be quizzed or invokes
/quiz by name.

One question at a time, don't move on until answered, no revealing the
answer early. Grade honestly (correct/partial/wrong), briefly explain misses
before the next question. Vary difficulty and angle. Default 8-10 questions
per session, close with a score summary and what to revisit.
```

**`eod`**: closes the day, what shipped, what's pending, one grounded closing line, a nudge to log before I close out. It also hands off to the [orchestrator](#the-orchestrator): a scoped cleanup of just today's files, then a small handoff file with tomorrow's priority that the orchestrator reads back on its next run.

```
# eod (Skill)
description: Use only when the user explicitly wraps up their day or invokes
/eod by name.

Check today's log and this week's file for open tasks, check today's
practice count, output shipped/pending/wins/tomorrow. One direct closing
sentence, not generic. Ask: "want to run /log before you close out?"

Then: scoped vault-audit cleanup limited to today's changed files (not a
full sweep). Write a handoff file with tomorrow's priority, overwritten
each night, for the orchestrator to check against the next morning.
```

#### Commands

Simple and deterministic enough that skill machinery would be overkill. Always explicit.

**`cmt` and `yur`**: make auto-commit a rule, not a suggestion. `cmt` stages and commits, `yur` does the same and pushes. My vault is closing in on a thousand commits this way.

```
# /cmt
Check git status/diff, pick the right type (feat/fix/refactor/docs/chore/
test/style/perf), write an imperative message under 72 characters, commit.
No AI attribution ever. One logical change per commit.

# /yur
Check for uncommitted changes (run /cmt first if needed), fetch, rebase on a
personal branch or merge on a shared one, stop on conflicts, push, confirm.
```

### Agents

Skills and commands run in my main chat. An agent doesn't, it's a separate Claude that goes off, does the whole job alone, and hands back just the result. Use one when the job reads across a ton of files or the web, runs long, and I only care about the outcome.

The [orchestrator](#the-orchestrator) is what wakes most of these up on a schedule, but each one below also runs standalone any time I call it directly.

- **research agent** — instead of me sitting in Google, opening tabs and copy-pasting, I hand it a topic and it does the whole dig: pulls from the best, highly-rated sources for that specific thing, cites them, and logs it back as a knowledge note using the conventions and folder structure it already knows from my vault. Isolated research in, a finished linked note in the right place out.
- **vault-audit agent** — my `lint` sweep reads the whole vault for stale notes, missing pages, broken links, drift. As an agent it just fixes the stale, obvious stuff on its own without asking, leans the list down using calls I've already made before, and only surfaces the handful of things that actually need my judgment. I get a short decision list, not a full audit dump. The orchestrator invokes it weekly; I can also run it standalone any time I want a deep sweep outside that cadence.
- **job-scout agent** — part job finder, part `lc-coach` for my whole career. It scours the most recent changes to anything FAANG-related in my vault, tells me the highest-ROI things to work on next so I stay focused, then hands back verified open roles plus a closing affirmation in the same voice the rest of the system runs on.

---

## Syncing across devices

> **Skip this whole section if you only ever work from one machine.**

- **Laptop:** Claude Code just points at the Drive-synced folder like any other directory. Open the same folder as an Obsidian vault on a second laptop and Drive keeps both in sync.
- **Phone:** more limited. Obsidian's mobile app can't open an arbitrary Drive-synced folder as a vault, that native experience needs Obsidian's own paid Sync (or a sync-focused plugin). Drive on a phone gets you the raw files plus claude.ai's Google Drive connector, not the native Obsidian UI.
- **Why Drive, not just GitHub:** a GitHub-only vault is just a repo of Markdown files, fine for the agent and for history, but not the Obsidian app. Obsidian's own cross-device sync is paid. Drive gets you that same experience on desktop for free.
- **Why GitHub still matters:** real version history, and a sanitized copy if you ever want to share your setup publicly.
- **Bonus:** persistent memory means switching devices doesn't mean rebuilding your agents, the same commands and preferences carry over into a fresh session.

---

## Notes

- **The vault outlasts any single agent.** Markdown and Git survive tool churn, when a better agent shows up, it's a folder path, not a migration.
- **`CLAUDE.md` is the highest-leverage file.** Everything downstream depends on it staying current.
- **Let the agent do the admin work.** Filing, linking, flagging stale notes, that's the busy work a self-maintaining vault is supposed to remove.
- **Public and private don't mix in one repo.** Keep a sanitized version separate from the vault with your real logs, goals, and personal data.
- **This is my scope, not the ceiling.** My focus right now is career and school, so that's what mine is built around, yours should be shaped around whatever you're chasing. I'm adding student-org and personal-interest skills as the semester goes, so this repo stays active. Swap in whatever MCPs, tooling, skills, and agents fit your life, and check back.

> Whatever you do with this is on you. Sensitive stuff especially, school records, other people's info, employer data, handle it however you're actually supposed to.
