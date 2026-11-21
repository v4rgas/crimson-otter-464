# AGENTS.md

Rules for each agent in this repo (Claude Code, Codex, OpenCode, and others).
Read this file first. Then read `context/README.md`.

## Required before any work

You cannot work without these two things. If one is missing, stop and tell your human.

1. **Ponytail is on.** At the start of each session, load the skill `ponytail`
   (`.agents/skills/ponytail/`) and keep it on for the full session.
2. **Discord access.** The `discord` MCP server works: `list_channels` shows
   `#tasks`, `#decisions`, `#releases`, and `#general`. Setup: `tools/discord-mcp/README.md`.

## Mission

We are in Platanus Hack 26 (Santiago), track **Simulations**.
We win with the pitch. The pitch is 3 minutes or less.
The pitch tells a short, strong, emotional story with one surprising moment.

The order of work is:

1. Choose the idea (the problem and the simulation).
2. Write the pitch. Build a full mock of the demo frontend.
3. Build backwards only what the demo needs.

We want 90% of the pitch ready on day one.

**How the simulation gets deep.** Our coding agents make the simulation deeper
during the hackathon, one layer per task. Each layer adds one mechanism or one
real data source, and must improve a fixed score against real data.
The demo shows the finished, deep simulation. The demo does NOT show AI agents
that improve the model live. See `context/decisions/20261121-0130-depth-grows-before-demo.md`.

## The golden rule

**If the demo does not show it, and nothing in the demo needs it, do not build it.**

- Make the simulation deeper. Do not add features.
- Before you start a task, find the moment in `context/pitch.md` that needs it.
  If no moment needs it, do not do the task.
- While `context/pitch.md` is empty, work only on: research for the problem,
  the mock, and the pitch.

## Work with minimum human input

Humans give input on only two things:

- **Taste:** what looks good and what goes in the pitch.
- **Direction:** if the product and the pitch go the correct way.

You decide all other things: tasks, technical design, code, data, tools.

- Do not ask humans technical questions. Decide, and write your reason
  in the pull request or in `context/decisions/`.
- Create, split, and take tasks yourself. Coordinate with other agents yourself.
- Ask humans only for taste or direction, and only in a release or at a checkpoint.

## Work rules

- **No tests.** It is a hackathon. All the effort goes into the product.
  Before a release, run the demo by hand and make sure that it works.
- **No defensive code.** Write only the happy path that the demo uses.
  Do not add fallbacks, retries, validation, or error handling for cases
  that the demo does not show.
- **Smallest code.** Ponytail is mandatory on every task.
  Use `ponytail-review` to review a change. Our rules win over ponytail: do not write tests or self-checks, and do not
  add validation or error handling that the demo does not need.
- **Research first.** Before you build, look for papers, libraries,
  open source code, and datasets (for example, open georeferenced data).
  Use what exists. Use the skill `research-first`.
- **Simple English.** Write all messages, pull requests, commits, and reports in
  Simplified Technical English. Use the skill `writing-simplified-technical-english`.
  Be short. Say only what the reader needs. No intro, no summary, no filler.
- **Frontend for a projector.** Use very little text. Do not use small text.
  Use large shapes, strong contrast, and clear motion.
- **Real data, not fake data.** The mock shows the look and the story.
  It is not the final product. If real data does not exist, tell the team
  in Discord and propose the nearest real option.

## Coordination

- All decisions and tasks go through Discord. Use the `discord` MCP tools.
  Your messages show your agent name, for example `quiet-heron (v4rgas@thinkpad)`.
- Tasks are posts in the `tasks` forum. Create one with `create_task`.
  Before you work on a task, read its thread and use `update_task` with `claimed`.
  Do not take a task that another agent claimed. If you need an answer, use `blocked`
  with the question. When you finish, use `done` with the pull request link.
- If a human gives you a decision in your session, post it to Discord and
  record it in `context/decisions/`.
- `main` is the shared branch. Do one task on one branch. Open small
  pull requests to `main`. Pull `main` frequently.
- If many agents run on one machine, use one git worktree and one dev server
  port for each agent.

## Releases

A release is a **version of the presentation**, not of the code.
It shows the pitch with the current state of the product.

Post each release to the Discord releases channel in this format:

```
Release vN: <one line, what changed in the story>
How to see it: <link or one command>
Screenshots: 1 to 3, only the key moments
Changed: 3 short lines or fewer
Question for humans: <one taste or direction question, or "none">
```

Do not post many screenshots or long logs.

## Checkpoints

At each checkpoint, humans review the last release and change the direction
if necessary. See `context/checkpoints.md`.

## Shared context

All durable context is in `context/`. `context/README.md` tells you how to
read it and how to add to it.
Skills are in `.agents/skills/`. `.claude/skills` points to the same folder.
