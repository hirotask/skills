---
name: github-issue-creator
description: Convert raw notes, error logs, voice dictation, or screenshots into crisp GitHub-flavored markdown issue reports. Use when the user pastes bug info, error messages, or informal descriptions and wants a structured GitHub issue. Supports images/GIFs for visual evidence.
---

# GitHub Issue Creator

Transform messy input (error logs, voice notes, screenshots) into clean, actionable GitHub issues.

## Output Template

- If `/.github/issue_template` directory exists at the repo root, use the template from that directory that matches the issue content.
- When you want to reference code from the codebase, paste the relevant GitHub link as-is; GitHub will display it as a preview, so make use of that.

## Output Location

**Create issues as markdown files** in `/issues/` directory at the repo root. Use naming convention: `YYYY-MM-DD-short-description.md`

## Guidelines

**Be crisp**: No fluff. Every word should add value.

**Extract structure from chaos**: Voice dictation and raw notes often contain the facts buried in casual language. Pull them out.

**Infer missing context**: If user mentions "same project" or "the dashboard", use context from conversation or memory to fill in specifics.

**Placeholder sensitive data**: Use `[PROJECT_NAME]`, `[USER_ID]`, etc. for anything that might be sensitive.

**Match severity to impact**:
- Critical: Service down, data loss, security issue
- High: Major feature broken, no workaround
- Medium: Feature impaired, workaround exists
- Low: Minor inconvenience, cosmetic

**Image/GIF handling**: Reference attachments inline. Format: `![Description](attachment-name.png)`

## Examples

**Input (voice dictation)**:
> so I was trying to deploy the agent and it just failed silently no error nothing the workflow ran but then poof gone from the list had to refresh and try again three times

**Output**:
```markdown

### Summary

- In summary, what is the issue about?
- What kind of requirements it is? (Answer via adding labels)

### The purpose of doing this development

- Who needs this feature/work to be done? In other word, who will benefit from the feature? (User/developer)
- Why the person needs it? (Because without it, (somebody) is not able to (do something)/Because with it, it would be much more efficient to ... -> Note: It has to be of business value)

### The degree/criteria of finish

- What is considered to be a sign/a criterion that the issue can be closed? (A clean criterion should contain a checklist of requirements, such that if the requirement is met the issue will be closed. This helps us to agree a common definition of "done". Usually you can use: Given (a context at the beginning of the scenario), when (the event that triggers the scenario), then (the expected outcome happens))
- When should the requirement be met?

### The idea to implement it

(If you have any idea to implement it, fill in this section.)
- What are the ideas you come up with?
- What could be done in order to close the issue? (The to-do list)

### Related issue

- Describe the prerequisite and the successive issues, as well as some similar requests.
```

