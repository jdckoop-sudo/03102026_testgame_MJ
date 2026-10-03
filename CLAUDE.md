# CLAUDE.md: rules for every Claude session in this repo

This repo is shared by **Jelle** (owner, GitHub `jdckoop-sudo`) and **Merijn**.
Both work on it with Claude, often at different times. GitHub is the **single
source of truth**: anything that is not pushed here does not exist.

Follow every rule below in every session. No exceptions, also not when asked to
"just quickly" do something.

---

## 1. Sync rules

### At the start of every session
1. Pull the latest version of the default branch (`main`) before answering anything.
2. Tell the user which version you are on: latest commit hash, message, author and time.
3. If anything changed since the user's last session (e.g. the other person pushed),
   give a short summary of what changed.

### Before every change
4. Pull again (`git pull --rebase`) right before editing. Never edit from a version
   that could be outdated, even if you pulled earlier in the same session.
5. Check `git status` is clean. If there are uncommitted changes you did not make,
   stop and ask.

### After every change
6. Commit immediately, one logical change per commit, with a clear message that
   says what changed and why.
7. Pull with rebase, then push. Do not collect changes to push later. A session
   can end at any moment and unpushed work is lost.
8. Confirm the push worked: show the commit hash that is now on GitHub
   (`git log origin/main -1`). If the push failed, say so clearly.
   **Never say something is saved when it is not pushed.**

### Never
9. Never `git push --force`, `git reset --hard` on pushed commits, rewrite history,
   or delete branches.
10. Never resolve a conflict on your own. If a pull, rebase or push gives a conflict
    (both people changed the same thing): **stop**, show both versions side by side,
    and let the user choose. Then commit the result and push.
11. If a push is rejected because the remote moved on: pull with rebase and try
    again. If that conflicts, see rule 10.

### End of session (user says "wrap up", "close", "stop", or similar)
12. Check that nothing is uncommitted or unpushed (`git status`, `git log origin/main..HEAD`).
13. Report the final state: the last commit on GitHub and what was done in this session.

---

## 2. Repo conventions

See `LEESMIJ.txt` for the full folder layout. In short:

```
00_File_Log/            JM_game_File_Log.xlsx: log of every file change
01_Ballistic_Bastion/
    Game_versions/YYYY-MM-DD_Vxx/   one folder per playable version (open index.html)
    Conversations/YYYY-MM-DD/       session summaries per day
    Changelog/                      Ballistic_Bastion_Changelog.xlsx: changes to the game itself
02_AI_Framework/
    Changelog/                      AI_Framework_Changelog.xlsx: changes to prompts and project content
    Prompts/
    Project_content/
```

- **Dates** in folder and file names are always `YYYY-MM-DD`.
- **New game version = new folder** in `Game_versions` with the next version number.
  Never overwrite or delete old version folders. The newest folder is the current game.
- **Every file change gets a row in `00_File_Log/JM_game_File_Log.xlsx`**
  (added, changed, renamed, moved, deleted), in the same commit as the change:
  - `Log` sheet: next `No`, date, time, `By` (Jelle / Merijn / Claude), folder,
    file name, action, old location (for renamed/moved), description, source,
    version, and the commit hash in `Notes` if known.
  - `Current files` sheet: add a row for a new file, remove the row for a deleted one.
  - Keep the existing formulas, dropdowns and formatting intact.
- Game changes (features, balance, bugs) also go in
  `01_Ballistic_Bastion/Changelog/Ballistic_Bastion_Changelog.xlsx`.
- Prompt and project-content changes also go in
  `02_AI_Framework/Changelog/AI_Framework_Changelog.xlsx`.

---

## 3. Working together

- Before starting, ask the user which part they are working on, so Jelle and
  Merijn do not edit the same file at the same time.
- Keep commits small and frequent. Small commits rarely conflict.
- If you see the other person changed something that affects the current task,
  mention it before continuing.
