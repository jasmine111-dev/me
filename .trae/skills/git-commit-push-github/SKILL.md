---
name: git-commit-push-github
description: 提交并推送当前项目到 GitHub 仓库。Use when the user asks to commit and push the current project to GitHub (github.com/jasmine111-dev/me). Do not use for Gitee pushes.
---

# Git Commit & Push to GitHub

Commit all changes and push to the GitHub remote repository, keeping the existing Gitee remote (`origin`) untouched.

## Prerequisite — SSH Key

This skill uses SSH authentication. The machine already has `C:\Users\PC-WP\.ssh\id_ed25519` (created 2026-10-15).

Public key (same key works for both Gitee and GitHub — add it once per platform):
```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIM9ZBLUZHxAGlCMi9IjjDHDk5R5PMe3c9QUJz6uQosSy gitee
```

**If GitHub push fails with "Permission denied (publickey)"**, tell the user:
1. Open https://github.com/settings/keys
2. Click New SSH key
3. Title: 魏鹏-PC  (or any label)
4. Key type: Authentication Key
5. Paste the public key above
6. Click Add SSH key
7. Retry the push

**If `ssh -T` fails with "port 22: Connection refused"** (common in China mainland), use GitHub's alternative port 443 by adding this to `C:\Users\PC-WP\.ssh\config`:
```
Host github.com
  Hostname ssh.github.com
  Port 443
  User git
```
Test again with `ssh -T git@github.com`.

## Workflow

1. Check status and diff:
   ```powershell
   git status
   git diff
   git log --oneline -5
   ```

2. Ensure the `github` remote is configured (separate from `origin` which points to Gitee):
   ```powershell
   git remote -v
   ```
   If `github` is missing, add it (SSH URL — not HTTPS):
   ```powershell
   git remote add github git@github.com:jasmine111-dev/me.git
   ```
   If `github` exists but points elsewhere, update it:
   ```powershell
   git remote set-url github git@github.com:jasmine111-dev/me.git
   ```

3. Test SSH connectivity (optional but recommended before first push):
   ```powershell
   ssh -T git@github.com
   ```
   Success: `Hi jasmine111-dev! You've successfully authenticated...`
   Failure: See "Prerequisite — SSH Key" above.

4. Stage all intended files:
   ```powershell
   git add -A
   ```

5. Commit with a concise message describing the changes:
   ```powershell
   git commit -m "描述本次变更内容"
   ```
   Rules:
   - Do NOT commit secrets or keys.
   - Do NOT skip hooks or use `--no-verify` unless explicitly requested.
   - Do NOT use interactive `-i` or force-push unless explicitly requested.
   - If commit fails with "Author identity unknown", ask the user for display name and email, then set repo-level config: `git config user.name "..." ; git config user.email "..."`.

6. Push to the GitHub remote (Windows PowerShell uses `;` as separator, not `&&`):
   ```powershell
   $branch = (git branch --show-current).Trim(); git push github $branch
   ```
   If push is rejected due to remote changes, pull first with rebase, resolve conflicts, then push again:
   ```powershell
   git pull --rebase github $branch
   git push github $branch
   ```

7. Report the result (commit hash, push status) to the user.

## Notes

- Gitee remote stays as `origin` → `https://gitee.com/wei-peng0328/weipeng.git`
- GitHub remote is `github` → `git@github.com:jasmine111-dev/me.git`
- Two remotes coexist; pushing to Gitee uses the existing `git-commit-push-gitee` skill.
- On Windows PowerShell, chain commands with `;` not `&&`.
- Verify push succeeded by checking output contains `<branch> -> <branch>` from GitHub.
