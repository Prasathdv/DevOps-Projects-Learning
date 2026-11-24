# Lessons Learnt

## Nov 23 2026 — Linux: Permissions

| # | What I did | What went wrong | Corrective step |
|---|---|---|---|
| 1 | Ran `chmod 750` / `600` on files | No effect on the permissions | The files were on the Windows filesystem. Moved to WSL `/home`, where Linux permissions work |
| 2 | Ran `git init` and committed inside `Phase1-permissions` | That folder became its own repo | No error yet; this caused problem #4 later |
| 3 | Created the GitHub repo with a README and `.gitignore` | GitHub now had a commit my laptop didn't have | No error yet; this caused problems #6 and #7 later |
| 4 | Ran `git add .` in `~/Learning` | Warning: *embedded git repository* (a repo inside a repo) | `git rm --cached -f` to unstage it, then `rm -rf …/Phase1-permissions/.git` to make it a normal folder |
| 5 | First `git push`, using email + password | *Password authentication is not supported* | Username `Prasathdv` + a Personal Access Token (scoped to this repo) |
| 6 | Pushed again | *Rejected (non-fast-forward)*: GitHub had the README commit | `git pull origin main --allow-unrelated-histories --no-rebase` |
| 7 | The pull merged the two histories | *CONFLICT* in `.gitignore`: both sides had one | Kept both versions, deleted the marker lines (including `HEAD` and the commit ID), then `git add` + `git commit` |
| 8 | Final `git push -u origin main` | Succeeded | `-u` remembers the target, so future pushes are just `git push` |

### Takeaways
- Keep all DevOps work in WSL `/home`, never in `/mnt/c`.
- Run `git init` **once**, at the top folder (`~/Learning`) only.
- For a repo whose code already exists locally, create the GitHub repo **empty** (no README) to skip steps 6–7.
- GitHub logins from the terminal use a **token**, not a password.
- Never `git push --force` to get past a rejection. Pull and merge instead.
