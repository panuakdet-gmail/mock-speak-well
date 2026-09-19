# speak-well

A skill for [Claude Code](https://claude.com/claude-code) that makes it **write answers you can read on a phone, in one go, without rereading a sentence.**

Short, but not clipped. The shortness comes from cutting whole ideas — the restated question, the caveat that changes nothing, the summary of what it just said — and then writing what survives in full, ordinary English. It does not come from squeezing sentences into fragments, which is what most "be concise" instructions actually produce.

It also bans four habits that make an answer tiring rather than long: hiding the verdict inside a noun instead of saying "this is wrong", explaining your own code and tools back to you, justifying a recommendation with something everybody already knows, and asking you a question it could have answered itself.

> **Claude Code** is Anthropic's coding assistant that runs in your terminal. A **skill** is a set of instructions you can hand it on demand, by typing a `/` command.

## The skill

| Command | What it does |
|---|---|
| `/mock-speak-well` | Rewrites the answer you just got so it is readable, then keeps writing that way for the rest of the session. |

It also switches itself on when you say an answer was too long, too dense, or hard to follow, or when you ask to have something said again more simply.

## What it actually changes

**Cut ideas, not words.** This is the rule the whole skill hangs on. Below, the first version is too long because it says things that did not need saying, the second is the wrong fix because it compressed the wording instead, and the third is right:

```
I went ahead and took a look at the configuration file, and it turns out that
there are a few different things going on here. First, the timeout value is
set quite low. It's worth noting that this may or may not be related to the
issue you're seeing, though it's probably worth checking. Additionally, ...
```

```
Config: timeout too low (5s). Poss. cause. Also retry=0 -> no recovery. Check both.
```

```
The timeout is set to 5 seconds, which is too short for this server, so the
request gives up before the reply arrives. Retries are switched off as well,
so nothing tries again afterwards.
```

**Say the verdict out loud.** You should never have to work out from a noun whether something is a complaint or a compliment. If it is a mistake, the answer says "this is wrong".

**Never explain your own work back to you.** You wrote the code and you chose the tool, so being told how they behave wastes your time.

**Never justify a choice with a truism.** "I would embed the picture in the note rather than only save the file, since a picture you cannot see is not much use" — the reason is true without knowing anything about your project, so it teaches you nothing and reads as condescending. Give the practical reason or give none.

**Guess instead of interviewing you.** Above 60% confidence, it picks the obvious option and tells you the assumption in one line. That threshold comes from its sibling skill, [minimal-edits](https://github.com/panuakdet-gmail/mock-minimal-edits).

**Leave the hard parts alone.** Error messages, stack traces, commands, file paths, exact flag names and numbers that matter are never simplified or shortened — they stay in code blocks, untouched, with the plain-English explanation written around them.

## What you need

- [Claude Code](https://claude.com/claude-code) — Anthropic's assistant for your terminal. Install it first; nothing here works without it.

That's the whole list. No other tools, no accounts, no configuration.

## Installing

Options 1 to 3 are for Claude Code. Option 4 is for other agents such as Codex or Gemini CLI. Pick one.

### Option 1 — ask Claude Code to do it

This is the easiest way, and you do not need a terminal. Open Claude Code and paste this:

```
Please install the Claude Code skills from https://github.com/panuakdet-gmail/mock-speak-well.

1. Download the repository, with its full history, to a temporary folder.
2. For each folder inside its skills/ folder, look for a folder with the same name in ~/.claude/skills/.
   - If there is none, copy the folder there.
   - If there is one, and every file in it matches a version that was once published in this repository, replace it with the new version.
   - If there is one, and it matches no published version, I have edited it. First move my copy to ~/.claude/skill-backups/<folder name>-<today's date>/. Then install the new version, show me what I had changed, and offer to merge my changes into it.
3. Delete the temporary download and tell me what you installed or updated.
```

Claude Code asks your permission before it changes anything. When it has finished, restart Claude Code.

**To update later**, paste the same prompt again. If you changed the skill yourself, your version is saved first, and Claude Code offers to merge your changes into the new one.

### Option 2 — install as a plugin

Choose this if you want Claude Code to handle updates for you. Inside Claude Code, run:

```
/plugin marketplace add panuakdet-gmail/mock-speak-well
/plugin install speak-well@speak-well-skills
```

To update later, run `/plugin marketplace update speak-well-skills`. Do not also use Option 1, otherwise the skill is installed twice.

### Option 3 — copy the folder yourself

In a terminal:

```bash
git clone https://github.com/panuakdet-gmail/mock-speak-well.git
mkdir -p ~/.claude/skills
cp -R mock-speak-well/skills/mock-speak-well ~/.claude/skills/
```

Then restart Claude Code. To update, delete the downloaded `mock-speak-well` folder and run the same commands again. This replaces the installed folder, so any changes you made to it are lost.

### Option 4 — ask another agent to do it

Open Codex, Antigravity, Gemini CLI, or whatever agent you use, and paste this:

```
Please install the skills from https://github.com/panuakdet-gmail/mock-speak-well for this agent.

1. Download the repository, with its full history, to a temporary folder.
2. Each folder inside its skills/ folder is one skill. Install each folder whole, including subfolders such as assets/, because the instructions use those files.
3. The skills were written for Claude Code, so adapt only the packaging: register each one the way this agent handles reusable skills or commands, so I can run it by name. Do not change the instructions inside each SKILL.md.
4. Before you install each skill, check whether I already have it.
   - If I do not, install it.
   - If I do, and its files match a version that was once published in this repository, replace it with the new version. Ignore packaging changes that an agent made when it installed the skill.
   - If I do, and it matches no published version, I have edited it. First copy my version to a backup folder outside the place this agent loads skills from, and tell me where it is. Then install the new version, show me what I had changed, and offer to merge my changes into it.
5. Delete the temporary download. Then tell me what you installed or updated, and how to run each skill here.
```

**To update later**, paste the same prompt again.

## Using it

You do not need to plan for it. Use it the moment an answer defeats you:

```
/mock-speak-well
```

It rewrites the previous answer and says nothing about having done so — no apology, no "here is a clearer version". Saying "that was too long to read" has the same effect.

An answer written under the skill looks like this. The verdict is in the first line, the reasons are specific to the project rather than general truths, and the decision you have to make is spelled out at the bottom with the exact words to reply:

```
Not difficult, about six lines.

The app already copies pictures into the vault and shows them in the note,
which is what happens when you forward a screenshot. The cover frame is in
memory at that same moment. It is simply not added to that list yet.

Two things worth knowing. Each frame is under a megabyte, but the frames go
into iCloud with the rest of the vault, so it grows slowly over time. Older
notes would not get one, because those frames were never saved and some of
those posts will be gone by now.

TL;DR: Easy, because the path already exists for screenshots. Reply
save the cover frames for new notes only, or save the cover frames and
backfill to also re-read the old pages.
```

To turn it off, say so — it stays on for the rest of the session otherwise.

### Asking for even shorter answers

Once the skill is on, it reads your messages for signs of how much you can take right now, and it checks every message, not only the first. These words do not switch the skill on by themselves:

| You write | You get |
|---|---|
| nothing special | the normal style described above |
| "concise", "concisely" or "สั้นๆ" | very concise answers: the verdict and the next step, usually two to four sentences |
| a word in ALL CAPS, "ffs", or an exclamation mark | the shortest answer possible, often one or two sentences, with every reason and side note cut |

Ordinary capitals, such as the start of a sentence or an acronym like "PDF", do not count. The shorter level stays on for the rest of the session until you say otherwise. At the two shorter levels, a fragment such as "Fixed." is allowed only when it cannot be misread, for example when the missing subject is obvious. Anything that could be read two ways is written as a full sentence.

## What it produces

Not files. The skill changes how the assistant writes for the rest of the conversation, and it checks its own answer against a short list before sending:

- Does the first line answer the question?
- Is anything here something you already know, or that would not change what you do next?
- Am I explaining your own code, tools or decisions back to you?
- Does any sentence justify a choice with a reason you could have written yourself?
- Where I judged something, did I say plainly that it is good or bad?
- Is every sentence a real sentence, with its connecting words intact?
- Would a tired person on a phone get through this in one go?

## Credits

The style is loosely modelled on **[ASD-STE100 Simplified Technical English](https://www.asd-ste100.org/)**, the controlled-English standard used for aerospace and defence maintenance manuals — one idea per sentence, everyday vocabulary, no ambiguity. It is owned by [ASD, the Aerospace and Defence Industries Association of Europe](https://www.asd-europe.org/standards-specifications/simplified-technical-english/), which publishes the specification free of charge; Issue 9 was released on 15 January 2025.

This skill borrows the spirit and none of the text. It uses no part of the STE dictionary or writing rules, it is not checked against them, and it is **not** STE-compliant — if you need real Simplified Technical English, go to the specification itself.

The 60% guess-don't-ask threshold is shared with [minimal-edits](https://github.com/panuakdet-gmail/mock-minimal-edits), by the same author.

## Licence

MIT — see [LICENSE](LICENSE). It covers the text in this repository. It says nothing about Simplified Technical English, which belongs to ASD, and nothing about the idea of writing plainly, which belongs to no one.
