# improve-skill

A Claude Code skill that fixes other skills from the problems they log while running.

## How it works

1. **Claude reports.** When any skill goes wrong (one of its scripts fails, an instruction can't be followed, Claude has to work around it, or you correct its result), Claude appends an entry to `~/.claude/skill-feedback/<skill>.md` and ends the run with one line pointing here. The steps are in `skills/improve-skill/references/reporting-problems.md`; one rule in your global `CLAUDE.md` turns them on for every skill, so skills don't need to change.
2. **You run `/improve-skill <skill>`.** It finds the skill's source (following symlinks to its repository) and works through the open entries. For a script problem it states the behaviour the script should have, writes or strengthens a test for that behaviour, confirms the test fails, and changes the script until the whole suite passes. For an instruction problem it makes the smallest wording change and explains why it works.
3. **You review and commit.** It marks each entry in the log and reports the before and after. It never commits; the changes are left in the skill's repository for you.

## Log entry format

```
## 2026-10-08 14:03 stage-lines.sh rejected a path with a tab
- skill: commit, step: Stage
- did: sh .../stage-lines.sh 'a<TAB>b' 2
- got: exit 2, No such file in the index or working tree
- expected: line 2 staged
- instead: staged the whole file with git add
```

`/improve-skill` adds `- status: ...` under each entry it handles.

## Install

Symlink or copy `skills/improve-skill/` to `~/.claude/skills/improve-skill/`, so it's available in every project. It runs only when you invoke it.

Then add to `~/.claude/CLAUDE.md`:

```markdown
## Skill problems

When a skill goes wrong itself (a script of its fails, an instruction can't be followed, you work around it, or I correct a result its instructions led to), follow `~/.claude/skills/improve-skill/references/reporting-problems.md`. Skip this if that file doesn't exist.
```
