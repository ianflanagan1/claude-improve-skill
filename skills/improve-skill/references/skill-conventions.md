# Skill conventions

How the user wants skills written. Apply these whenever you create or change a skill, by any route: by hand, with skill-creator, or with another skill. Where they differ from skill-creator's defaults, these win. Where the skill's own repository consistently does something different, follow the repository.

## Status line

A skill's reply starts with one status line when its run ends in one of these outcomes. Write each in SKILL.md as the exact text in backticks: the emoji, a space, and a short sentence ending in a full stop.

The command-line tools in `~/projects/cli` end with the same status lines, through `cli-kit/src/cli_kit/outcome.py`; change the two together. A skill that runs one of those commands can repeat the command's status line as its own.

| Emoji | Outcome | Example |
|---|---|---|
| ✅ | The operation changed state as asked | `✅ Committed.` |
| 🛑 | It stopped part way, after changing some state | `🛑 Stopped.` |
| ❌ | It ran and failed | `❌ Commit failed.` |
| ⚠️ | A precondition wasn't met, so nothing was tried | `⚠️ Uncommitted changes.` |
| ➖ | There was nothing to do | `➖ Nothing staged.` |

- Use only the outcomes the skill can have. ✅, 🛑 and ❌ are for skills that change state (commit, push, publish, edit files). A skill that only produces text, such as one suggesting a commit message, has no ✅ line: its output is the result. ⚠️ and ➖ fit any skill's early exits.
- After ✅: the result in as few lines as it takes (new commits, a URL), and nothing else.
- After 🛑: what was done, what's left, and how to undo or resume.
- After ❌: the error, unchanged, in a fenced `text` code block, then one short line on the cause and the fix you propose. Don't apply the fix unless the skill says to.
- After ⚠️: the precondition that failed and what the user should do about it.
- After ➖: nothing. Make no tool calls once you know there's nothing to do.
