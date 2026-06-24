---
name: commit
description: >
  Stage and commit changes using well-structured Git commit messages.
  Use this skill whenever the user wants to commit code, says "/commit", asks you to
  "コミットして", "変更をコミットして", "差分をコミットして", or any variation of
  "commit this", "commit the changes", or "git commit". Also trigger when the user asks
  you to save work to Git or wrap up changes. Split into logical units automatically —
  don't wait to be asked. This skill enforces the project's commit message conventions
  (English subject, Japanese body, imperative mood, What + Why).
---

# Commit Skill

## Overview

Inspect the current diff, break it into meaningful commit units, stage each unit, write a
message following the commit rules below, and commit. Repeat until nothing remains to commit.

## Workflow

### 1. Get the diff

```bash
git diff
git diff --staged   # also check what's already staged
git status
```

### 2. Plan commit units

Think through the diff and decide how to split it into small, focused commits. Each commit
should represent one logical change — the simpler and more self-contained the better.
Avoid mixing unrelated concerns in a single commit.

One rule: the Subject line must not contain a `+` sign.

### 3. Stage the right files

Use `git add <files>` (or `git add -p` for partial staging) to stage exactly the files
that belong to this commit unit.

### 4. Write the commit message

Follow the rules in [Commit Rules](#commit-rules) below. Think about:
- **Subject**: What changed, in one short imperative sentence (English, ≤50 chars)
- **Body**: Why it was changed and what the impact is (Japanese)

### 5. Commit

```bash
git commit -m "Subject

Body line 1
Body line 2"
```

The Body is **required** — omitting it is not acceptable. Always explain the *why*.

### 6. Repeat

If there are still uncommitted changes, return to step 3 and continue until `git status`
shows a clean working tree (or no further relevant changes).

---

## Commit Rules

Based on [How to Write a Git Commit Message](https://cbea.ms/git-commit/).

### Seven Rules

| Rule | Detail |
|------|--------|
| Separate subject and body | One blank line between them |
| Limit subject to 50 chars | Keeps it scannable |
| Capitalize the subject | First letter uppercase |
| No period at end of subject | Clean, headline-style |
| Imperative mood in subject | "Fix bug" not "Fixed bug" or "Fixes bug" |
| Wrap body at 72 chars | For terminal readability |
| Body explains What and Why | Not How — the code shows How |

### Project-Specific Rules

- **Subject**: Write in **English**
- **Body**: Write in **Japanese**
- The 72-char wrap for the body is a best-effort goal for Japanese text (which doesn't use
  word-based wrapping), not a hard requirement

### What NOT to include

- Do **not** add any auto-generated footers

---

## Example

```
Add user authentication endpoint

ログインAPIを追加した。
セッション管理にJWTを採用することで、サーバーサイドのセッションストアが
不要になり、スケールアウト時の複雑さを減らせる。
```
