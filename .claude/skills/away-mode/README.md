# Away Mode: handoff note for the desktop session

Written on 2026-10-06 from a cloud Claude Code session while Johnny was on a flight, unable to see the blitz campaign session running on his home PC.

## What happened

- The desktop Claude Code session was running the blitz campaign.
- Remote Control had not been turned on in that session, so it was invisible from the phone.
- Chrome Remote Desktop was blocked by the in-flight network.
- Nothing had been pushed to GitHub, so there was no other way to read progress.
- A cloud session has no path to a home PC. No credential or permission changes that.

## What to do the moment you are back at the PC

Takes under two minutes. Do these in the terminal or Desktop window where the blitz session is running.

1. **Review the blitz output first.** It kept running the whole time. Read what it produced before changing anything.

2. **Turn Remote Control on for this session.** Type:
   ```
   /rc "Blitz campaign"
   ```
   The session appears in the Claude mobile app under the Code tab within a minute.

3. **Make it the default for every future session.** Type `/config` and enable "Enable Remote Control for all sessions". Or add this to `~/.claude/settings.json`:
   ```json
   { "remoteControlAtStartup": true }
   ```

4. **Install this skill so every project has it.** Copy the folder to your user-level skills directory:
   ```
   mkdir -p ~/.claude/skills
   cp -r .claude/skills/away-mode ~/.claude/skills/
   ```
   After that, `/away-mode` works in any Claude Code session on this machine.

5. **Run it once now.** Type `/away-mode` in the blitz session. Claude will set the default, confirm Remote Control, and start pushing a `PROGRESS.md` trail to a `wip/` branch so GitHub shows progress even when the PC is asleep.

## What Remote Control needs to stay alive

- The PC stays on and awake. Disable sleep before a trip.
- The terminal or Desktop window stays open.
- You are logged in with your claude.ai account, not an API key.

If the session drops, `claude remote-control --continue` in the same folder brings back the last session for roughly four hours after it stopped.

## Where to look on the phone

Claude app > Code tab. Online sessions show a computer icon with a green dot. The same list is at claude.ai/code in any browser.

## Reference

https://code.claude.com/docs/en/remote-control
