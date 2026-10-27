# Switching between Claude accounts

The owner works on this repo from several Claude accounts and switches at any time. Accounts share **nothing but Git**:
no chat, no memory, no project files. A step that is not committed and pushed does not exist for the next account.

## Rules

1. **One source of truth:** branch `main` of `JIMMAN-TUE/dashboard-migration`. Status lives in `HANDOFF.md`, history in `LOG.md`.
2. **One writer at a time:** the *Working now* line in `HANDOFF.md` is the lock. Value: `nobody` or `<chat name> · <YYYY-MM-DD HH:MM Bangkok> · <task>`.
3. **Chat names:** `PBI-A` (account 1), `PBI-B` (account 2), `PBI-C` (account 3). Put the name in every lock and log line.
4. **Small, tested commits; push after each.** `git pull --rebase` before every push. Never force-push, never rewrite history.
5. **Handoff in the same commit as the change** (status, table map, checklist, problems, *Next step*) + one `LOG.md` line.
6. **Owner results go into Git:** errors, refresh results, totals, decisions pasted in chat are written into `HANDOFF.md` before the turn ends.
7. **Original first:** a report's untouched PBIP is committed before any edit, so every change is a diff and can be reverted.
8. **No secrets anywhere:** no keys, passwords, tokens in files, commits or chat. Credentials live only in Power BI / gateway settings.
9. **Never remove** anything the owner asked for earlier; the owner only Pulls / opens / refreshes, never edits code.

## Take over (start of every task)

1. `git fetch origin main && git pull --rebase origin main`.
2. Read `HANDOFF.md` (whole) and the last 10 lines of `LOG.md`.
3. *Working now* not `nobody` → stop and ask the owner (another account may be working; a stale lock is cleared only by the owner's word).
   `nobody` → write your claim, commit `lock: <chat> <task>`, **push at once**. Push rejected → pull, read the lock again; if someone claimed first, stop and tell the owner.
4. Work from *Next step*.

## Hand over (end of every task, or owner says "switch" / "บันทึก" / "ปลดล็อก" / `/handover`)

1. Commit only tested work. Unfinished: commit what is safe and write exactly where it stopped + the next command in *Next step*.
2. Update `HANDOFF.md` + one `LOG.md` line in that commit.
3. Set *Working now* to `nobody`, commit `unlock: <chat>`, `git pull --rebase`, push; check `git status` is clean and `git rev-parse HEAD` = `origin/main`.
4. Tell the owner in Thai: commit id, what is done, what to type in the next account (prompt at the end of `HANDOFF.md`).

## New account / machine (once)

1. Claude: connect GitHub as `JIMMAN-TUE` (or a collaborator with push rights); the Claude GitHub App must cover this repo.
2. Local machine (optional): clone **outside OneDrive**, one folder per account; `git config user.name/user.email`.
3. Paste the prompt at the end of `HANDOFF.md`.
