---
name: analyze-pr-comments
description: Analyze PR review comments from technical perspective
allowed-tools: Bash(gh pr view), Bash(gh pr view --comments), Bash(gh pr view --json number), Bash(gh repo view --json owner,name), Bash(gh api repos/*/pulls/*/comments)
---

# Analyze PR Comments Command

This command analyzes review comments for the current branch's pull request from a technical perspective.

The goal is to evaluate code review feedback objectively, considering both the technical merit of suggestions and their practical value in the context of the project.

## Current PR Status

!gh pr view

## PR Comments Overview

!gh pr view --comments

## Get Repository Information

First, get the owner and repository name:

!gh repo view --json owner,name

## Get PR Number

!gh pr view --json number

## Code Review Comments Analysis

Replace OWNER, REPO, and PR_NUMBER with actual values from the commands above:

!gh api repos/OWNER/REPO/pulls/PR_NUMBER/comments

## Analysis

For each review comment, provide:

- Pros: Benefits of the proposed change
- Cons: Drawbacks and risks of the proposed change  
- Assessment: Technical evaluation based on the Pros/Cons analysis

Remember: Every technical proposal has both positive and negative aspects. Analyze both sides objectively.

Provide assessment in Japanese (polite form).
