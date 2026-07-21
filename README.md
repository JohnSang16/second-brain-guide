# obsidian + claude code: a student's second brain

obsidian holds your notes as linked markdown. claude code reads and writes to them as an agent. together, you get a knowledge base that updates itself instead of one you maintain by hand. built and used daily as a cs student running an internship, a student org, classes, and a faang-prep grind at once, but the pattern holds for anyone who wants an agent that actually knows their life instead of starting from zero every chat.

- [the idea](#the-idea): why obsidian plus claude code beats either one alone
- [quick setup](#quick-setup): two installs, that's the whole technical lift
- [syncing across devices](#syncing-across-devices): google drive for desktops, what your phone actually gets
- [scaffolding from zero](#scaffolding-from-zero): let claude code interview you, then migrate what you already have
- [my setup](#my-setup): the actual folder structure
- [how i actually use it](#how-i-actually-use-it): the daily-full secretary, plus my most-used agents
- [end notes](#end-notes): what mattered more than the folders themselves

---

## the idea

claude code already reads your whole working folder as context. what it doesn't have is a durable, agent-agnostic place to keep what it learns about you, somewhere structured enough that you can browse it too.

obsidian is that place:

- **notes are markdown.** same format claude code already reads and writes.
- **the vault is just a folder.** point claude code at it, it has full context. no uploads, no separate memory system to babysit.
- **`[[wiki-links]]` connect notes.** easy for an agent to create and follow, easy for you to browse in the graph view.
- **it's agent-agnostic.** the vault outlives whatever tool you're using it with today.

obsidian holds the memory. claude code reads it, writes to it, acts on it.

**why this matters as a student:** you're context-switching all day, classes, internship, clubs, job search, and re-explaining your situation to an ai every session is wasted time. with a vault, the agent already knows your goals and your active threads and can pull it together on command.

---

## quick setup

two installs and you're ready to go:

1. **obsidian** (free): [obsidian.md](https://obsidian.md/download)
2. **claude code**: [claude.com/product/claude-code](https://claude.com/product/claude-code) for install and terminal/ide setup

make your vault its own folder, open claude code from that same folder. that's the whole technical setup. everything past this point is structure and habit, not tools.

---

## syncing across devices

hosting the vault in google drive is what actually made this usable across my laptops.

- **if you only ever work from one machine, skip this.**
- on a laptop, claude code points at the local drive-synced folder like any other directory. nothing special about it. open the same folder as an obsidian vault on a second laptop and drive keeps both showing the same notes.
- on a phone, it's more limited, and worth being straight about. the obsidian mobile app can't just open an arbitrary drive-synced folder as a vault, that native experience needs obsidian's own paid sync (or a similar sync-focused plugin). what drive does give you on a phone is access to the raw files through the drive app, and claude.ai's google drive connector can query straight into the vault from there, just not through the native obsidian mobile ui.

here's the actual reason drive matters, not just github: if the vault only lived in a github repo, you'd have a stack of markdown files you can browse in vs code or on github.com. useful for the agent, useful for history, but that's not the obsidian app. obsidian's own cross-device sync is a paid feature. hosting the vault in google drive gets you the desktop version of that for free, drive syncs the folder across your laptops, and obsidian just opens that synced folder as a vault on whichever machine you're on. you get the real obsidian ui, graph view and all, on every desktop, without paying for obsidian sync.

push the vault to github too, it's solving a different problem: real version history, and a sanitized copy if you ever want to share your setup publicly. drive is what makes the vault feel like one continuous app across desktop machines, github is what makes it durable and shareable.

one more reason this pays off: with persistent memory, switching devices doesn't mean rebuilding your agents and context from scratch. the same commands and preferences carry over into a fresh claude session on whatever device you're on, instead of you re-explaining your setup every time.

---

## scaffolding from zero

don't hand-design the folder structure up front. let claude code interview you instead. ask about your goals, your commitments, how you actually think, then have it draft a structure and get your approval before it writes anything.

starter prompt:

> i want to use obsidian to help you and me better manage context. the vault is at [folder path]. ask me about my goals, responsibilities, and how i want to work with you, one question at a time. once we're aligned, propose a folder structure and starter files. wait for my approval before creating anything.

the one file that actually matters is **`CLAUDE.md`** at the vault root. claude code reads it at the start of every session. it should define:

- who you are and what you're working on (link out to a profile/goals note, don't cram it in-line)
- the vault's folder structure, so the agent knows where things go
- writing conventions (naming, linking, tone)
- any standing behaviors you want automated (auto-commit, daily briefs, whatever)

everything downstream, daily logs, knowledge notes, slash commands, comes out of that one file staying accurate.

### already sitting on a pile of notes?

chances are you're not starting from a blank vault. for me it was a pile of google docs i'd been dumping stuff into for a while with no real structure. that's exactly where connecting the google drive mcp earns its keep: point claude code at the existing docs, and it pulls them into the vault as markdown notes and reshapes them into your structure, instead of you copy-pasting for a weekend.

this is honestly the main advantage of running claude code with mcps wired in: what would've been a multi-day migration chore becomes one conversation. i did exactly this, connected the drive mcp, told claude code to bring the docs over and restructure them, and it just handled it.

---

## my setup

```
vault/
├── CLAUDE.md              # entry point, read every session
├── HOME.md                 # daily cockpit / quick links
├── _meta/                  # identity + goals the agent references constantly
├── memory/                 # claude code's persistent cross-session memory
├── work/                    # internship/job: daily logs, knowledge base, people, projects
├── career/                  # faang prep, leetcode tracker, resume, linkedin drafts
├── org/                     # club or student org: projects, people, knowledge
├── personal/                 # finances, fitness, personal projects
└── .claude/commands/         # slash commands, mirrored in _agents/commands/ by category
```

those trailing comments describe what's inside each folder (`work/` breaks down into its own `daily/`, `knowledge/`, `people/`, and `projects/` subfolders, and so on), not extra root-level items. only the lines above are actually at the vault root.

a few decisions that mattered more than the folder names:

- **knowledge notes are separate from daily logs.** daily logs are a stream of what happened. knowledge notes are distilled, linked, written for "me in six months," and filed into topic subfolders (`work/knowledge/cs/web`, `.../security`, etc). the daily brief flags things worth promoting into a knowledge note instead of letting them rot in a log.
- **`memory/` is not a task list.** it holds durable facts about how i work and what's been decided, not in-progress state. tasks live in actual planning tools. memory is for things that should still be true next month.
- **commands are organized twice on purpose.** a flat `.claude/commands/` folder so claude code can find them, a categorized `_agents/commands/<category>/` mirror so a human browsing the repo can find them too. keeping two copies in sync is a real cost, worth it for readability.

---

## how i actually use it

### `/daily-full`: the secretary that runs every morning

this is the anchor of the whole system, and my actual main use case. every morning it acts like a secretary or chief of staff: it doesn't hand me raw data, it pulls from every source at once and hands back a single synthesized brief of what actually matters today.

**what it pulls, via mcp tooling:** messaging (slack), email (gmail, filtered to work and recruiting, everything promotional gets skipped), a github check, a calendar look-ahead (google calendar, next 7 days), a few tracker files that live in the vault itself (a leetcode tracker, a career priority dashboard), and a live web search for open roles at companies i actually care about.

**what makes it different from a dashboard:** it applies day-aware filtering before writing anything, weekdays get full context, weekends filter down to personal and career only, sunday adds a monday-prep section. then it writes like a briefing, not a report, sections only show up if there's real content, nothing gets repeated across sections.

**the customization layer:** i've tuned the exact tone of the closing affirmation (mine reads as hype-man energy, talks directly to "you" not "today," ties real numbers back to the goal instead of using them as the point) and the exact shape of the output template. none of that is fixed, it's meant to be rewritten in your own voice for your own goals. here's a trimmed, genericized version of the actual command:

```
# /daily-full

runs the full daily brief. operate in deep work mode. do not just report data,
synthesize it. think about what day it is, what's actually at stake today, and
what the single most important thing to do is. the brief should read like a
chief of staff who already did the thinking, not a dashboard that dumped the data.

---

## step 1: pull sources in parallel

- messaging (slack via mcp): full threads only, flag if genuinely unresolved
  and needs a reply today
- email (gmail via mcp): work and recruiting related only, skip anything promotional
- github: flag only if action is needed
- calendar (google calendar via mcp): next 7 days
- a personal tracker file: streak, weekly count, next focus area
- a career priority-dashboard file: anything due within 30 days
- a live web search for open roles at target companies, verified and still open

## step 2: apply day-aware filtering before writing anything

- weekdays: full context, filter for what's highest leverage today specifically
- weekend day one: personal and career only, skip routine work unless truly urgent
- weekend day two: same as above, plus a next-workday prep section

## step 3: output format

no dashboard tables, no status rows. write like a briefing, not a report.
sections only appear if they have real content, skip empty ones entirely.
each item appears exactly once, in the single most relevant section.

[main goals]          -> fixed, rarely changes
[identity]             -> fixed affirmation block, shown verbatim every time
[start here]           -> today's single highest-priority focus area
[non-negotiables]      -> the 2-3 things that must happen today
[move the needle]      -> 2-4 concrete, day-aware actions
[messages]             -> only if a reply is genuinely needed today
[crucial reminders]    -> only deadlines inside 7 days
[open roles]           -> verified listings worth applying to

## step 4: affirmation

3-4 sentences after the brief, tuned to whatever tone actually works for you.
talk directly to the reader, "you," not "today." real numbers belong here as
fuel, not as the point of the sentence. no generic motivational filler.
```

### my agents

past the daily secretary, these are the commands i actually reach for most, in roughly the order i use them. each one gets a quick overview, then the real (trimmed, genericized) system prompt behind it.

**`/log`** feeds anything, a learning, a recap, a reflection, straight into the vault. it classifies the input and files it without me having to decide where it goes, this is the single most-used command in my setup by a wide margin.

```
# /log

takes raw input, a learning, a session recap, a standup update, a reflection,
anything, and files it in the right place. no questions asked. read the input,
classify it, route it.

## classify

| type               | destination                                    |
|--------------------|-------------------------------------------------|
| technical learning | a knowledge note in the right topic subfolder    |
| daily activity      | today's daily log, work section                 |
| personal            | today's daily log, personal section              |
| mix                 | split and file each part                         |

## route

- knowledge note: check if this extends an existing note or needs a new one.
  extending: append a new section, preserve everything already there.
  new: create it with a source line, a related-notes link, and the content.
- daily log: append to today's file if it exists, create it from the template if not.
- commit immediately after writing either one.

## confirm

one sentence per file touched. state the path and what was added. nothing else.
```

**`/lint`** runs every two weeks and audits the vault instead of me having to notice what's gone stale on my own.

```
# /lint

run every 2 weeks. audits the vault for gaps, stale items, and missed documentation.

## checks

- knowledge gaps: scan daily logs for concepts mentioned but never given a knowledge note
- people gaps: cross-reference names mentioned against people pages, flag anyone missing one
- stale items: flag anything in a todo-style folder not updated in over 14 days
- empty folders: flag folders that should have content by now but don't
- stale paths: check CLAUDE.md against the real folder structure, flag mismatches
- pattern topics: flag anything mentioned across 3+ separate days, that deserves its own note
- broken links: flag any [[link]] pointing to a file that doesn't exist yet
- knowledge index: regenerate the index file, grouped by folder

## output

a short itemized report, one line per category, then ask: "want me to create
any of these missing files now?"
```

**`/cmt` and `/yur`** make auto-commit a rule instead of a suggestion. `/cmt` stages and commits, `/yur` does the same and pushes. my own vault is closing in on a thousand commits this way.

```
# /cmt

stages and commits current changes using conventional commits.

1. check git status and diff to see exactly what changed
2. pick the right type: feat, fix, refactor, docs, chore, test, style, perf
3. write an imperative, present-tense message under 72 characters
4. commit, then confirm the hash and summary

no ai attribution in commit messages, ever. one logical change per commit.

# /yur

ensures everything is committed, the remote is synced, and pushed in one shot.

1. check for uncommitted changes, run /cmt first automatically if there are any
2. fetch and check if the remote has moved
3. rebase on a personal branch, merge on a shared branch
4. stop and ask if a conflict shows up, never resolve one automatically
5. push, confirm the branch and commit hashes that landed
```

**`/lc-coach`** wraps my leetcode practice in a status brief, a gamified xp and streak layer, and a closing affirmation, so it feels like a habit i want to keep instead of a chore.

```
# /lc-coach

the gamified practice session captain. wraps a standard interview-simulation
protocol in an opening status brief, a gamification layer, and a closing affirmation.

## opening status brief

streak, weekly count, overall progress through a fixed problem set, today's
assigned problem, and an open "free play" lane for anything else that sounds fun.

## two modes

- ranked match: exactly one per day, the full formal interview protocol, end to
  end. this is the only mode that actually flips a problem to "solid" in the tracker.
- free play: unlimited, casual, lightly logged, no formal protocol required. the
  point is volume of genuine engagement, not compliance.

## gamification

xp per solve, a bonus for solving clean, a bonus for harder problems, a
persistent rank ladder tracked in a tracker file, and a visual progress bar so
the remaining list visibly shrinks session over session.

## tone

nice and supportive, always, regardless of how the session goes. high
standards and "never give the answer directly" don't soften, but the delivery
stays warm and patient, this is the one deliberate carve-out from the rest of
my setup's default directness.
```

**`/tutor`** re-explains something we already covered, at the plainest level possible, for whenever an explanation landed too technical or too fast.

```
# /tutor

re-explains something already discussed in this conversation at the plainest,
most digestible level. use when an explanation landed too technical, too
dense, or too fast.

## usage

- /tutor: re-explain my own most recent substantive explanation
- /tutor last 20 lines: re-explain a specific slice instead
- /tutor <topic>: re-explain just that piece, even if it wasn't the most recent thing said

## how to respond

1. start at the highest level, plain language first, define every term on first use
2. use analogies grounded in something real or verifiable, never a made-up hypothetical
3. split multi-part explanations into short, clearly labeled sections
4. actually re-derive the explanation, don't just repeat the same wording
5. no meta-commentary about what changed, just give the clearer version

conversation-only, nothing gets written to a file unless i separately say to log it.
```

**`/eod`** closes the day: what shipped, what's pending, one grounded closing line, then a nudge to log the day before i close out.

```
# /eod

runs the end-of-day wrap.

1. check today's daily log and this week's file for open tasks
2. check today's practice/problem count
3. output the eod dashboard: shipped, pending, wins, tomorrow's top priority
4. one closing sentence, direct, grounded in where i actually am, not generic
5. ask: "want to run /log to capture today in full before you close out?"
```

---

## end notes

- **the vault is worth more than any single agent.** markdown and git mean it survives tool churn. when a better agent shows up, it's a folder path, not a migration.
- **`CLAUDE.md` is the highest-leverage file in the whole system.** everything else is downstream of it staying current. review and edit it directly, don't let it drift.
- **let the agent do the admin work.** filing, linking, flagging stale notes, that's exactly the busy work between having an idea and executing on it that a self-maintaining vault is supposed to remove. if you're manually tagging and organizing, you've recreated the problem obsidian plus agent was supposed to solve.
- **public and private don't mix in one repo.** if you're sharing your setup (like this guide), keep a sanitized version separate from the vault that has your actual daily logs, goals, and personal data in it.
- **this is my scope, not the ceiling.** swap in whatever mcps, tooling, skills, and agents fit your own life, the pattern holds even when the folders don't.
