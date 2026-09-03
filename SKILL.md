---
name: task-tracker
description: "Persistent cloud task tracker via GitHub repo + TASKS.md. Add, update, complete, and push tasks with full history."
---

# Task Tracker

Manage tasks through a single markdown file in a GitHub repo. Every change is committed and pushed for cloud persistence and history.

## Repo

- Local path: `/home/nitr0gen/task-tracker`
- Remote: `https://github.com/mouimet-infinisoft/task-tracker.git`
- Task file: `TASKS.md`

## TASKS.md Format

Every task has a unique ID prefix before the checkbox. IDs are immutable once assigned. Each task line ends with two spaces to force a Markdown line break.

```markdown
# Tasks

## Todo
T1 - [ ] Task description  
T2 - [ ] Another task  

## In Progress
T3 - [ ] Currently working on this  

## Done
T4 - [x] Completed task  
```

### Format rules

- ID goes BEFORE the checkbox: `T{n} - [ ]`, never `[T{n}]`
- Every task line must end with exactly two trailing spaces
- Section headers: `## Todo`, `## In Progress`, `## Done`
- Empty sections are omitted

## Commands

### List tasks

```bash
cat /home/nitr0gen/task-tracker/TASKS.md
```

### Add task

Append under `## Todo` with next available ID, formatted `T{n} - [ ] description  `.

### Update task by ID

When updating or deleting, match by ID text, not description. Edit or remove the line beginning with `T{n} - [`.

### Complete task

Move the `T{n} - [ ]` line to `## Done` and change checkbox to `- [x]`.

### Email retrieval workflow

When the user says an email contains task details, use `himalaya` directly:
1. `himalaya envelope list --json` to find the message ID by subject/from/date.
2. `himalaya message read <ID>` to extract details.
3. Update the task with extracted facts; do not ask the user to paste content.

### Commit and push

```bash
cd /home/nitr0gen/task-tracker && git add TASKS.md && git commit -m "Describe change" && git push
```

## References

- `references/task-format.md` — exact task-line templates and update-by-ID rules
- `references/reminder-cron.md` — voice reminder cron jobs for Slack/TTS
- `references/email-retrieval.md` — reading emails with `himalaya` and extracting task details
- `references/conventions.md` — existing conventions

## Rules

1. Always commit and push after any task change.
2. Keep descriptions short and actionable.
3. Use sections: `Todo`, `In Progress`, `Done`.
4. If a section is empty, omit it.
5. IDs are assigned sequentially and never reused or changed.
6. Always update/delete by ID.
7. When task details come from email, retrieve them directly with `himalaya` rather than asking the user to paste content.
8. When rewriting `TASKS.md` fully, verify all existing tasks are preserved before committing.
9. Voice reminders must be delivered across all available channels. Use `deliver: "all"` when creating or updating reminder cron jobs so they remain accessible regardless of session, context, or communication channel.
