---
name: away-mode
description: Make this Claude Code session visible and steerable from Johnny's phone before he leaves the computer, and keep progress visible from GitHub even if Remote Control drops. Use when he says "away mode", "I'm leaving", "I'm travelling", "flight", "make this visible from my phone", "turn on remote control", or asks how to follow progress remotely. Also use at the start of any long-running campaign or batch job.
---

# Away Mode

Goal: Johnny can see and steer this session from the Claude mobile app, and can read progress from GitHub even when the session itself is unreachable.

Background: on 2026-10-06 a desktop session ran a blitz campaign while Johnny was on a flight. Remote Control was never turned on, Chrome Remote Desktop was blocked, and nothing had been pushed. A cloud session could not reach the PC, so the work ran blind for the whole flight. This skill prevents that.

## Step 1: Turn on Remote Control for every future session

Set the user-level default so this never has to be remembered again.

Read `~/.claude/settings.json` first. If it does not exist, create it. Merge in this key without removing anything else:

```json
{
  "remoteControlAtStartup": true
}
```

Confirm to Johnny that it is set. Equivalent manual paths, if he prefers:

- In any session: `/config`, then enable "Enable Remote Control for all sessions"
- Claude Desktop: Settings > Claude Code > "Connect new sessions to Remote Control"

## Step 2: Turn on Remote Control for THIS session

The setting above only affects sessions started after it is saved. The current session must be connected by hand, and Claude cannot run built-in slash commands on his behalf. Tell him to type exactly this, with a meaningful name:

```
/rc "Blitz campaign"
```

`/rc` is the alias for `/remote-control`. The session then appears in the Claude mobile app under the Code tab, with a computer icon and a green dot while online, and at claude.ai/code in any browser.

Tell him the two conditions that keep it alive: the computer must stay on and awake, and this terminal or Desktop window must stay open. Closing either ends the remote connection.

If Johnny cannot type into this session because it is mid-task, tell him it is safe to wait for the next prompt. Typing `/rc` does not cancel running work.

## Step 3: Make progress visible from GitHub as a fallback

Remote Control needs the PC awake and online. GitHub does not. Before Johnny leaves, set up a visible trail:

1. Confirm the working directory is a git repo with an `origin` remote he can read from his phone. If not, say so and skip to Step 4.
2. Create or switch to a work-in-progress branch named `wip/<short-task-name>`.
3. Create `PROGRESS.md` at the repo root with this shape:

```markdown
# <Task name> progress

Last updated: <ISO timestamp>
Status: running | blocked | done

## Done
- ...

## In progress
- ...

## Next
- ...

## Blocked / needs Johnny
- ...
```

4. Commit and push now so the branch exists remotely.
5. For the rest of the task, after every meaningful milestone and at least every 30 minutes of active work, update `PROGRESS.md`, commit everything on the branch, and push. One commit per milestone. Never squash or rewrite while he is away.
6. Never commit secrets, credentials, or customer personal data to the progress branch. Reference them by file name only.

## Step 4: Optional mobile push notifications

If he wants to be notified when the session needs him, tell him to run `/config` and enable mobile push notifications. This only works once Remote Control is on.

## Step 5: Confirm the checklist out loud

Before he leaves, report exactly these five lines, filled in:

```
Remote Control default: set / not set
This session on phone:  connected as "<name>" / waiting for you to type /rc
Progress branch:        wip/<name> pushed / no git remote
PROGRESS.md:            created and pushed / skipped
Push cadence:           every milestone, max 30 min
```

## What to do if he messages from the road

If a message arrives through Remote Control while a task is running, answer it, then continue the task. If asked for status, read `PROGRESS.md` and report the Done, In progress, and Blocked sections verbatim. Do not restart or re-plan the task because he checked in.

## Prerequisites and known blockers

- Remote Control needs a claude.ai login on Pro, Max, Team, or Enterprise. API-key logins and Bedrock, Vertex, or Foundry endpoints do not support it.
- The env vars `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` and `DISABLE_GROWTHBOOK` disable Remote Control entirely. If `/rc` fails, check for these first.
- Chrome Remote Desktop and similar screen-sharing tools are not needed and are not a substitute. Remote Control is the supported path.
- A cloud Claude Code session (claude.ai/code "cloud" sessions) cannot reach this computer. Only software already running here can expose this session.

Official reference: https://code.claude.com/docs/en/remote-control
