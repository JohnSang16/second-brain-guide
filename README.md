# Obsidian + Claude Code: A Student's Second Brain

Obsidian holds your notes as linked Markdown. Claude Code reads and writes to them as an agent. Together, you get a knowledge base that updates itself instead of one you maintain by hand. Built and used daily as a CS student running an internship, a student org, classes, and a FAANG-prep grind at once, but the pattern holds for anyone who wants an agent that actually knows their life instead of starting from zero every chat.

- [The idea](#the-idea): why Obsidian plus Claude Code beats either one alone
- [Quick setup](#quick-setup): two installs, that's the whole technical lift
- [Syncing across devices](#syncing-across-devices): Google Drive for desktops, what your phone actually gets
- [Scaffolding from zero](#scaffolding-from-zero): let Claude Code interview you, then migrate what you already have
- [My setup](#my-setup): the actual folder structure
- [How I actually use it](#how-i-actually-use-it): daily briefs, logging, lint, LeetCode coaching, school agents
- [End notes](#end-notes): what mattered more than the folders themselves

---

## The idea

Claude Code already reads your whole working folder as context. What it doesn't have is a durable, agent-agnostic place to keep what it learns about you, somewhere structured enough that you can browse it too.

Obsidian is that place:

- **Notes are Markdown.** Same format Claude Code already reads and writes.
- **The vault is just a folder.** Point Claude Code at it, it has full context. No uploads, no separate memory system to babysit.
- **`[[wiki-links]]` connect notes.** Easy for an agent to create and follow, easy for you to browse in the graph view.
- **It's agent-agnostic.** The vault outlives whatever tool you're using it with today.

Obsidian holds the memory. Claude Code reads it, writes to it, acts on it.

**Why this matters as a student:** you're context-switching all day, classes, internship, clubs, job search, and re-explaining your situation to an AI every session is wasted time. With a vault, the agent already knows your goals and your active threads and can pull it together on command.

---

## Quick setup

Two installs and you're ready to go:

1. **Obsidian** (free): [obsidian.md](https://obsidian.md/download)
2. **Claude Code**: [claude.com/product/claude-code](https://claude.com/product/claude-code) for install and terminal/IDE setup

Make your vault its own folder, open Claude Code from that same folder. That's the whole technical setup. Everything past this point is structure and habit, not tools.

---

## Syncing across devices

Hosting the vault in Google Drive is what actually made this usable across my laptops.

- **If you only ever work from one machine, skip this.**
- On a laptop, Claude Code points at the local Drive-synced folder like any other directory. Nothing special about it. Open the same folder as an Obsidian vault on a second laptop and Drive keeps both showing the same notes.
- On a phone, it's more limited, and worth being straight about. The Obsidian mobile app can't just open an arbitrary Drive-synced folder as a vault, that native experience needs Obsidian's own paid Sync (or a similar sync-focused plugin). What Drive does give you on a phone is access to the raw files through the Drive app, and claude.ai's Google Drive connector can query straight into the vault from there, just not through the native Obsidian mobile UI.

Here's the actual reason Drive matters, not just GitHub: if the vault only lived in a GitHub repo, you'd have a stack of Markdown files you can browse in VS Code or on github.com. Useful for the agent, useful for history, but that's not the Obsidian app. Obsidian's own cross-device sync is a paid feature. Hosting the vault in Google Drive gets you the desktop version of that for free, Drive syncs the folder across your laptops, and Obsidian just opens that synced folder as a vault on whichever machine you're on. You get the real Obsidian UI, graph view and all, on every desktop, without paying for Obsidian Sync.

Push the vault to GitHub too, it's solving a different problem: real version history, and a sanitized copy if you ever want to share your setup publicly. Drive is what makes the vault feel like one continuous app across desktop machines, GitHub is what makes it durable and shareable.

One more reason this pays off: with persistent memory, switching devices doesn't mean rebuilding your agents and context from scratch. The same commands and preferences carry over into a fresh Claude session on whatever device you're on, instead of you re-explaining your setup every time.

---

## Scaffolding from zero

Don't hand-design the folder structure up front. Let Claude Code interview you instead. Ask about your goals, your commitments, how you actually think, then have it draft a structure and get your approval before it writes anything.

Starter prompt:

> I want to use Obsidian to help you and me better manage context. The vault is at [folder path]. Ask me about my goals, responsibilities, and how I want to work with you, one question at a time. Once we're aligned, propose a folder structure and starter files. Wait for my approval before creating anything.

The one file that actually matters is **`CLAUDE.md`** at the vault root. Claude Code reads it at the start of every session. It should define:

- who you are and what you're working on (link out to a profile/goals note, don't cram it in-line)
- the vault's folder structure, so the agent knows where things go
- writing conventions (naming, linking, tone)
- any standing behaviors you want automated (auto-commit, daily briefs, whatever)

Everything downstream, daily logs, knowledge notes, slash commands, comes out of that one file staying accurate.

### Already sitting on a pile of notes?

Chances are you're not starting from a blank vault. For me it was a pile of Google Docs I'd been dumping stuff into for a while with no real structure. That's exactly where connecting the Google Drive MCP earns its keep: point Claude Code at the existing docs, and it pulls them into the vault as Markdown notes and reshapes them into your structure, instead of you copy-pasting for a weekend.

This is honestly the main advantage of running Claude Code with MCPs wired in: what would've been a multi-day migration chore becomes one conversation. I did exactly this, connected the Drive MCP, told Claude Code to bring the docs over and restructure them, and it just handled it.

---

## My setup

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
└── .claude/commands/         # slash commands, mirrored in _agents/commands/ by category
```

Those trailing comments describe what's inside each folder (`work/` breaks down into its own `daily/`, `knowledge/`, `people/`, and `projects/` subfolders, and so on), not extra root-level items. Only the lines above are actually at the vault root.

A few decisions that mattered more than the folder names:

- **Knowledge notes are separate from daily logs.** Daily logs are a stream of what happened. Knowledge notes are distilled, linked, written for "me in six months," and filed into topic subfolders (`work/knowledge/cs/web`, `.../security`, etc). The daily brief flags things worth promoting into a knowledge note instead of letting them rot in a log.
- **`memory/` is not a task list.** It holds durable facts about how I work and what's been decided, not in-progress state. Tasks live in actual planning tools. Memory is for things that should still be true next month.
- **Commands are organized twice on purpose.** A flat `.claude/commands/` folder so Claude Code can find them, a categorized `_agents/commands/<category>/` mirror so a human browsing the repo can find them too. Keeping two copies in sync is a real cost, worth it for readability.

---

## How I actually use it

### `/daily-full` is the anchor of the whole system

Everything above exists to feed this one command. It's a full morning brief that pulls calendar, email, and messages, then cross-references trackers and priority dashboards that live in the vault, LeetCode progress, career-prep priorities, active projects, into a single filtered view of what actually matters today. Not a raw digest of every input.

Here's why it's worth building the rest of the vault around: since it's backed by durable notes instead of a blank chat, the brief already knows my active goals, my in-progress threads, and what I've already flagged as done or deprioritized. It gets sharper every week instead of repeating the same generic advice, and it does the "reassemble my whole situation" work that would otherwise eat the first ten minutes of every morning. It's also where introspection and goal alignment actually happen day to day, asking it "what should I focus on" gets an answer grounded in commitments already on file, not a generic productivity answer. If you only build one thing from this guide, build the version of this command that fits your life.

### The logging workflow: how pages actually get added

In practice this routes through one command, `/log`. Feed it anything, a learning, a recap, a reflection, and it classifies the input and files it into the right daily-log section or knowledge-note subfolder without you having to think about where it goes. This is the single most-used command in my vault by a wide margin, hundreds of commits and counting.

The standard loop for adding to the vault, and this matters more than any folder structure:

- **Learning something new:** don't let the agent replace the thinking. Read the material, actually learn it, then log a short note to the vault after. The value isn't the note itself, it's that you now know you've touched the topic before. Weeks later, `/tutor` or a plain query against the vault pulls that note back up instead of you re-learning it from scratch or forgetting you ever covered it.
- **Logging daily work:** if you're at an internship or job, logging what you did each day is one of the highest-value habits here. Just be careful with anything NDA-covered or confidential. Keep those entries high-level, the kind of problem, not proprietary specifics, or skip logging the sensitive part entirely and rely on your own memory for that. Even without specifics, the vault still works as a rough index of "when did I work on X."
- **Logging people:** new coworkers, coffee chats, people met at a new org or internship. A running page per person means months later you're not trying to reconstruct who someone was from a two-line memory.
- **Logging inspiration:** anything that sparks an idea, a post, an approach, worth a quick note even with no immediate use. It compounds. Searching the vault later for "have I seen something like this before" actually works because you wrote it down when it happened.

Put together, this is the best note-taking app I've used, not because of any single feature, but because the notes aren't inert. They're linked, queryable, and the agent proactively pulls them into a brief or a coaching session instead of you having to go dig them out yourself.

### Other commands worth the pattern

**LeetCode coaching (`/lc`, `/lc-coach`):** a structured coaching protocol that never hands over the answer directly, tracks streak and session state in a tracker file, and stays encouraging regardless of how the session goes. Consistency in tone across sessions is something a plain chat can't give you, a vault-backed command can.

**School-specific agents (`/tutor`, `/quiz`, `/hw-check`):** coursework support that reads what I'm actually taking and where I'm stuck from vault notes, instead of generic tutoring. `/hw-check` in particular is just a review pass against my own submitted work before I turn it in.

**Vault maintenance (`/lint`):** runs every two weeks, scans daily logs for knowledge gaps, checks for people who were mentioned but never got a page, flags coffee chats gone stale, catches broken `[[links]]` and empty folders, and regenerates the knowledge index. This is the actual mechanic behind letting the agent do the admin work, I don't go looking for the rot, the command finds it and hands me a punch list.

**Auto-commit as an audit trail (`/cmt`, `/yur`):** every meaningful vault change gets its own commit, `/cmt` stages and commits, `/yur` does the same and pushes. Sounds like overkill until you realize it turns the vault into a real history of how your thinking and priorities evolved, not just a static snapshot. My own vault is closing in on a thousand commits this way.

---

## End notes

- **The vault is worth more than any single agent.** Markdown and Git mean it survives tool churn. When a better agent shows up, it's a folder path, not a migration.
- **`CLAUDE.md` is the highest-leverage file in the whole system.** Everything else is downstream of it staying current. Review and edit it directly, don't let it drift.
- **Let the agent do the admin work.** Filing, linking, flagging stale notes, that's exactly the busy work between having an idea and executing on it that a self-maintaining vault is supposed to remove. If you're manually tagging and organizing, you've recreated the problem Obsidian plus agent was supposed to solve.
- **Public and private don't mix in one repo.** If you're sharing your setup (like this guide), keep a sanitized version separate from the vault that has your actual daily logs, goals, and personal data in it.
- **This is my scope, not the ceiling.** Swap in whatever MCPs, tooling, skills, and agents fit your own life, the pattern holds even when the folders don't.
