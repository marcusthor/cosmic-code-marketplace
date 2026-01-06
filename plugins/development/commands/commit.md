---
description: Create well-formatted commits following Conventional Commits specification
argument-hint: [--no-verify]
---

# Git Commit

I'll create a well-formatted commit following the Conventional Commits specification.

## Step 1: Analyze Current State

Using the **commit** skill to:
- Check staged and unstaged changes
- Review the diff to understand what changed
- Check recent commits for style consistency

$ARGUMENTS

## Step 2: Determine Commit Strategy

Based on the analysis:
- If changes are atomic: Create single commit
- If changes are mixed: Suggest splitting into multiple commits
- If nothing staged: Stage relevant files first

## Step 3: Create Commit

Format: `<type>(<scope>): <description>`

- Use imperative mood ("add" not "added")
- Keep first line under 72 characters
- Match repository commit style

**Starting commit analysis...**
