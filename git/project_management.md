# GitHub Project Management for Small Teams

## 1. Branch Strategy

Never work directly on `main`. Always branch off it.

```
main          → stable, production-ready code only
dev           → integration branch (merge features here first)
feature/xxx   → your working branch
fix/xxx       → for bug fixes
```

**Workflow:**
1. Pull the latest `dev` before starting anything
2. Create your branch from `dev`
3. Work, commit, push
4. Open a Pull Request (PR) into `dev`
5. Someone else reviews and merges
6. `dev` → `main` only when everything is stable and tested

```bash
git checkout dev
git pull origin dev
git checkout -b feature/my-new-feature
```

## 2. Commit Format

Use this structure for every commit:

```
<type>(<scope>): <short description>

[optional body — explain WHY, not WHAT]
```

### Types

| Type       | When to use                          |
|------------|--------------------------------------|
| `feat`     | New feature                          |
| `fix`      | Bug fix                              |
| `docs`     | Documentation only                   |
| `style`    | Formatting, no logic change          |
| `refactor` | Code restructure, no behaviour change|
| `test`     | Adding or updating tests             |
| `chore`    | Build scripts, dependencies, config  |

### Examples

```
feat(auth): add password reset flow

fix(cart): prevent duplicate items on fast click

docs(readme): update local setup instructions

chore(deps): upgrade lodash to 4.17.21
```

**Rules:**
- Use the imperative tense: *"add"* not *"added"* or *"adds"*
- Keep the first line under 72 characters
- One logical change per commit — don't bundle unrelated things

## 3. Pull Requests

### Opening a PR
- Give it a clear title using the same format as commits
- Add a short description: what changed and why
- Link any related issue (`Closes #12`)
- Assign at least one reviewer

### Reviewing a PR
- Check it out locally if it's non-trivial
- Leave specific, constructive comments
- Approve only when you're actually happy with it
- Don't leave PRs open for more than 2 days — review or say when you will

### Merging
- Prefer **Squash and merge** for feature branches (keeps `dev` history clean)
- Delete the branch after merging

## 4. Avoiding Conflicts

Most conflicts come from two people editing the same file. Reduce the risk:

- **Communicate before starting** — a quick message like *"I'm working on the navbar today"* saves hours
- **Keep branches short-lived** — merge within 1–3 days if possible
- **Pull `dev` regularly** into your branch while you work:

```bash
git fetch origin
git rebase origin/dev
```

- **Split work by file/module** when possible — don't all edit `App.js` at once

If you do get a conflict:
1. Don't panic
2. Open the file, look for `<<<<<<<` markers
3. Decide which change to keep (or combine both)
4. Stage and continue: `git add . && git rebase --continue`

## 5. Issues & Project Board

Use GitHub Issues to track work — even for a small team, it prevents things from falling through the cracks.

### Issue format

```
Title: [type] Short description
Body:
  - What needs to be done
  - Why it matters
  - Acceptance criteria (what "done" looks like)
```

### Project Board (GitHub Projects)

Set up 4 columns:

| Backlog | In Progress | In Review | Done |
|---------|-------------|-----------|------|

- Move your issue to **In Progress** when you start
- Move to **In Review** when your PR is open
- Close the issue when the PR is merged

## 6. Quick Reference

```bash
# Start new work
git checkout dev && git pull origin dev
git checkout -b feature/your-feature

# Save progress
git add .
git commit -m "feat(scope): description"
git push origin feature/your-feature

# Stay up to date
git fetch origin
git rebase origin/dev

# Clean up after merge
git checkout dev
git pull origin dev
git branch -d feature/your-feature
```

>[!IMPORTANT]
>**Golden rule:** If you're unsure whether your change might affect someone else's work — ask first, push second.
