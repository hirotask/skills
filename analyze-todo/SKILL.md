---
name: analyze-todo
description: >
  Collect, categorize, and summarize TODO comments found in source code.
  Use this skill whenever the user says "/analyze-todo", asks to "TODOを調べて",
  "TODOを一覧化して", "コード内のTODOを確認して", "技術的負債を洗い出して",
  or any variation of "find TODOs", "list TODOs", or "what's in the backlog".
  Also trigger when the user wants to turn TODOs into GitHub Issues — run this
  skill first to gather the list, then hand off to the issue skill.
argument-hint: "対象ディレクトリやファイルパターン（省略時はプロジェクト全体）"
---

# Analyze TODO Skill

## Overview

Search the codebase for TODO comments, categorize them, and output a prioritized list.
Optionally propose converting them to GitHub Issues via the `issue` skill.

## Workflow

### 1. Determine scope

If the user provided an argument (directory path or file pattern), restrict the search to
that scope. Otherwise search the entire project.

### 2. Collect TODOs

Run a grep that covers all common comment styles:

```bash
grep -rn \
  --include="*.ts" --include="*.tsx" --include="*.js" --include="*.jsx" \
  --include="*.py" --include="*.rb" --include="*.go" --include="*.java" \
  --include="*.css" --include="*.html" --include="*.md" --include="*.sql" \
  -iE "(TODO|FIXME|HACK|XXX)(\([^)]*\))?:" . \
  | grep -v "node_modules" \
  | grep -v "\.git" \
  | grep -v "dist" \
  | grep -v "build"
```

Patterns to capture:
- `// TODO: ...` — JS / TS / Java / Go / C / C++
- `# TODO: ...` — Python / Ruby / Shell / YAML
- `<!-- TODO: ... -->` — HTML / XML / Markdown
- `/* TODO: ... */` — CSS / C-style
- `-- TODO: ...` — SQL
- `TODO(name): ...` — assigned TODOs

Also treat `FIXME`, `HACK`, and `XXX` as high-priority variants.

### 3. Categorize and prioritize

For each found item, infer:

- **Category**: Bug fix / Feature addition / Refactoring / Documentation / Test addition / Other
- **Priority**: High (`FIXME`, `HACK`, `XXX`, or urgent wording) / Normal (`TODO`) / Low (cosmetic/docs)

### 4. Output the list

```
## TODO 一覧

| # | ファイル | 行 | 内容 | カテゴリ | 優先度 |
|---|----------|----|------|----------|--------|
| 1 | src/foo.ts | 42 | キャッシュ処理を実装する | 機能追加 | 通常 |
| 2 | src/bar.py | 87 | エラーハンドリングを追加 | バグ修正 | 高 |
...

## サマリー
- 合計: N 件
- バグ修正: N 件
- 機能追加: N 件
- リファクタリング: N 件
- その他: N 件
```

### 5. Suggest next actions

After outputting the list, suggest:

- Use the `issue` skill to register specific TODOs as GitHub Issues
- Ask whether to create one Issue per TODO or group related ones together

**Example prompts to hand off:**

```
# 特定の TODO を Issue 化する
/issue analyze-todo の結果にある「キャッシュ処理を実装する」(src/foo.ts:42) をもとに Issue を作成して

# 複数の TODO をまとめて Issue 化する
/issue analyze-todo の結果にあるリファクタリング系 TODO をまとめて1つの Issue にして
```
