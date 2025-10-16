---
name: Code Review Checklist
description: Automated code review workflow for self-review before commits
when_to_use: before committing code, requesting PR reviews, or performing self-review
version: 1.0.0
---

# Code Review Checklist

## Overview

Systematic code review checklist to catch common issues before committing or requesting review.

**Core principle:** Verify quality, security, and maintainability through structured checks.

## The Checklist

### 1. Tests
- [ ] Tests exist for new functionality
- [ ] All tests pass locally
- [ ] Edge cases are covered
- [ ] Test names clearly describe what they verify

**Run tests:**
```bash
# Adjust command for your project
npm test
# or pytest
# or go test ./...
```

### 2. Security
- [ ] No hardcoded secrets (API keys, passwords)
- [ ] User input is validated
- [ ] SQL queries use parameterization (no string concatenation)
- [ ] Authentication/authorization checks in place

**Quick check for secrets:**
```bash
grep -r "api_key\|password\|secret" --include="*.js" --include="*.py" .
```

### 3. Error Handling
- [ ] Errors are caught and handled appropriately
- [ ] Error messages are helpful (not just "error occurred")
- [ ] No silent failures
- [ ] Logging includes context for debugging

### 4. Code Quality
- [ ] Functions are small and focused
- [ ] Variable names are clear and descriptive
- [ ] No commented-out code
- [ ] Consistent with project style

### 5. Documentation
- [ ] README updated if behavior changed
- [ ] Comments explain "why", not "what"
- [ ] Complex logic has explanatory comments
- [ ] API changes documented

## Quick Review Process

1. Run tests
2. Check security issues
3. Review error handling
4. Verify code quality
5. Update documentation

## Remember

- This is a basic checklist, not exhaustive
- Adapt to your project's needs
- Automate checks where possible
- Use as starting point for team standards
