---
name: github-pr-draft
description: >
  Create a well-structured GitHub Draft PR from the current branch against develop.
  Use this skill whenever the user says "/pr-draft", "PRを作って", "プルリクを作成して",
  "PR のタイトルと説明を考えて", "draft PR を出して", or any variation of
  "open a pull request", "create a PR", or "submit for review". Also trigger when
  the user asks to prepare PR description or fill in the PR template — handle the
  full workflow from diff analysis to `gh pr create --draft`.
---

# PR Draft Skill

## Overview

Inspect the diff between the current branch and `develop`, draft a PR title and
description following the project's rules, confirm with the user, then create a Draft PR
via the `gh` CLI.

## Workflow

### 1. Gather repo context

```bash
git remote -v
git branch --show-current
```

### 2. Analyze the diff

```bash
git diff develop..HEAD
git log develop..HEAD --oneline
```

Read through the diff carefully to understand what changed and why.

### 3. Check for a related Issue (optional)

If the branch name contains `#<number>` (e.g. `feature/#42-add-login`), that number is
the Issue number. Fetch it:

```bash
gh issue view <number>           # title, body, labels
gh issue view <number> --comments
```

Use the Issue context to write a more accurate PR description.

### 4. Draft title and description

Follow the rules in [PR Rules](#pr-rules) below. Think through:
- **Title**: one-line summary of the change in natural English
- **Description**: fill the template (project template if it exists, fallback below)

Check for a project-level PR template first:

```bash
cat $(pwd)/.github/pull_request_template.md 2>/dev/null
```

If the template exists, fill it in. If not, use the [Fallback Template](#fallback-template).

### 5. Confirm with the user

Present the drafted title and description in a readable format and ask for approval or
corrections before creating the PR. This step is important — don't skip it.

### 6. Create the Draft PR

```bash
gh pr create --draft --title "<title>" --body "$(cat <<'EOF'
<description>
EOF
)"
```

---

## PR Rules

### Title

- Write in natural English
- The title alone should convey the PR's purpose
- Capitalize the first word
- No trailing period

### Description

Use the project's `.github/pull_request_template.md` when it exists. Otherwise use the
[Fallback Template](#fallback-template) below.

#### Fallback Template

```markdown
### 概要

このPRの概要を書きます

### 関連Issue

- close #{Issue番号}

### レビュー観点

- [ ] （具体的なレビューポイント）

### レビュー手順

- [ ] （具体的な確認手順）

### 補足情報

レビュワーに伝えなければならない情報をここに記載します。
```

### Review points guideline

To keep reviewer burden low:
- Avoid open-ended questions — write specific, checkable items
- Minimize the number of external documents to reference
- Make checklist items concrete, not abstract ("Is column X set correctly?" not "Check the data")
