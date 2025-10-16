# Claude Code Marketplace Implementation Plan

> **For Claude:** Use `${SUPERPOWERS_SKILLS_ROOT}/skills/collaboration/executing-plans/SKILL.md` to implement this plan task-by-task.

**Goal:** Create a reference implementation marketplace with 3 plugins (code-review, api-dev, refactoring) demonstrating Claude Code plugin structure.

**Architecture:** Git-based marketplace with JSON manifests, containing 3 independent plugins. Each plugin bundles a slash command that references a skill. Plugins are opt-in (not auto-enabled).

**Tech Stack:** JSON for configuration, Markdown for commands/skills, Git for distribution.

---

## Task 1: Create Marketplace Root Structure

**Files:**
- Create: `manifest.json`
- Create: `README.md`
- Create: `.gitignore`

**Step 1: Create marketplace manifest**

Create `manifest.json`:

```json
{
  "name": "demo-marketplace",
  "version": "1.0.0",
  "description": "Reference implementation for Claude Code marketplace",
  "author": "Your Team",
  "repository": "https://github.com/yourorg/demo-marketplace",
  "plugins": [
    {
      "id": "code-review",
      "name": "Code Review",
      "description": "Automated code review checklist",
      "version": "1.0.0",
      "path": "plugins/code-review"
    },
    {
      "id": "api-dev",
      "name": "API Development",
      "description": "REST API scaffolding guide",
      "version": "1.0.0",
      "path": "plugins/api-dev"
    },
    {
      "id": "refactoring",
      "name": "Refactoring Assistant",
      "description": "Safe refactoring workflow",
      "version": "1.0.0",
      "path": "plugins/refactoring"
    }
  ]
}
```

**Step 2: Create marketplace README**

Create `README.md`:

```markdown
# Demo Claude Code Marketplace

Reference implementation demonstrating Claude Code marketplace and plugin structure.

## Installation

Add this marketplace to Claude Code:

\`\`\`bash
/plugin marketplace add <your-git-url>
\`\`\`

## Available Plugins

### Code Review
Automated code review checklist for self-review before commits.

**Install:** \`/plugin add code-review\`
**Usage:** \`/review\`

### API Development
REST API scaffolding guide with standard patterns.

**Install:** \`/plugin add api-dev\`
**Usage:** \`/api\`

### Refactoring Assistant
Safe refactoring workflow with test-driven approach.

**Install:** \`/plugin add refactoring\`
**Usage:** \`/refactor\`

## Plugin Structure

Each plugin contains:
- \`plugin.json\` - Plugin metadata
- \`commands/*.md\` - Slash commands
- \`skills/SKILL.md\` - Workflow skills

## For Learning

This marketplace demonstrates:
- Marketplace manifest structure
- Plugin organization and metadata
- Slash command + skill integration
- Opt-in plugin activation

## License

MIT
\`\`\`

**Step 3: Create .gitignore**

Create `.gitignore`:

```
# OS files
.DS_Store
Thumbs.db

# IDE
.vscode/
.idea/

# Temp files
*.log
*.tmp
```

**Step 4: Verify structure**

Run: `ls -la`

Expected output showing:
- manifest.json
- README.md
- .gitignore
- docs/

**Step 5: Commit marketplace foundation**

```bash
git init
git add manifest.json README.md .gitignore docs/
git commit -m "feat: initialize marketplace structure"
```

---

## Task 2: Create Code Review Plugin

**Files:**
- Create: `plugins/code-review/plugin.json`
- Create: `plugins/code-review/commands/review.md`
- Create: `plugins/code-review/skills/SKILL.md`

**Step 1: Create plugin directory structure**

Run: `mkdir -p plugins/code-review/commands plugins/code-review/skills`

**Step 2: Create plugin metadata**

Create `plugins/code-review/plugin.json`:

```json
{
  "name": "code-review",
  "version": "1.0.0",
  "description": "Automated code review checklist",
  "author": "Your Team",
  "commands": ["review"],
  "skills": ["code-review"],
  "autoEnable": false,
  "resources": {
    "commands": {
      "review": "commands/review.md"
    },
    "skills": {
      "code-review": "skills/SKILL.md"
    }
  }
}
```

**Step 3: Create slash command**

Create `plugins/code-review/commands/review.md`:

```markdown
@skills/code-review/SKILL.md
```

**Step 4: Create skill**

Create `plugins/code-review/skills/SKILL.md`:

```markdown
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
\`\`\`bash
# Adjust command for your project
npm test
# or pytest
# or go test ./...
\`\`\`

### 2. Security
- [ ] No hardcoded secrets (API keys, passwords)
- [ ] User input is validated
- [ ] SQL queries use parameterization (no string concatenation)
- [ ] Authentication/authorization checks in place

**Quick check for secrets:**
\`\`\`bash
grep -r "api_key\|password\|secret" --include="*.js" --include="*.py" .
\`\`\`

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
\`\`\`

**Step 5: Verify plugin structure**

Run: `ls -R plugins/code-review/`

Expected output:
```
plugins/code-review/:
commands/  plugin.json  skills/

plugins/code-review/commands:
review.md

plugins/code-review/skills:
SKILL.md
```

**Step 6: Commit code-review plugin**

```bash
git add plugins/code-review/
git commit -m "feat: add code-review plugin with /review command"
```

---

## Task 3: Create API Development Plugin

**Files:**
- Create: `plugins/api-dev/plugin.json`
- Create: `plugins/api-dev/commands/api.md`
- Create: `plugins/api-dev/skills/SKILL.md`

**Step 1: Create plugin directory structure**

Run: `mkdir -p plugins/api-dev/commands plugins/api-dev/skills`

**Step 2: Create plugin metadata**

Create `plugins/api-dev/plugin.json`:

```json
{
  "name": "api-dev",
  "version": "1.0.0",
  "description": "REST API scaffolding guide",
  "author": "Your Team",
  "commands": ["api"],
  "skills": ["api-dev"],
  "autoEnable": false,
  "resources": {
    "commands": {
      "api": "commands/api.md"
    },
    "skills": {
      "api-dev": "skills/SKILL.md"
    }
  }
}
```

**Step 3: Create slash command**

Create `plugins/api-dev/commands/api.md`:

```markdown
@skills/api-dev/SKILL.md
```

**Step 4: Create skill**

Create `plugins/api-dev/skills/SKILL.md`:

```markdown
---
name: API Development Guide
description: REST API scaffolding with standard patterns
when_to_use: when building new API endpoints or structuring API projects
version: 1.0.0
---

# API Development Guide

## Overview

Standard patterns for building REST APIs with consistent structure, error handling, and validation.

**Core principle:** Convention over configuration with clear, predictable API design.

## REST Endpoint Structure

### Standard HTTP Methods

- **GET** - Retrieve resources (read-only, idempotent)
- **POST** - Create new resources
- **PUT** - Replace entire resource
- **PATCH** - Update partial resource
- **DELETE** - Remove resource

### URL Patterns

```
GET    /api/resources          # List all
GET    /api/resources/:id      # Get one
POST   /api/resources          # Create
PUT    /api/resources/:id      # Replace
PATCH  /api/resources/:id      # Update
DELETE /api/resources/:id      # Delete
```

## Response Format

### Success Response (200, 201)

```json
{
  "data": {
    "id": "123",
    "name": "Example",
    "created_at": "2025-10-15T12:00:00Z"
  }
}
```

### Error Response (400, 404, 500)

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input data",
    "details": [
      {
        "field": "email",
        "issue": "Invalid email format"
      }
    ]
  }
}
```

### Status Codes

- **200 OK** - Successful GET, PUT, PATCH, DELETE
- **201 Created** - Successful POST
- **400 Bad Request** - Invalid input
- **401 Unauthorized** - Missing/invalid authentication
- **403 Forbidden** - Insufficient permissions
- **404 Not Found** - Resource doesn't exist
- **500 Internal Server Error** - Server error

## Request Validation

### Validate Early

```javascript
// Example validation middleware
function validateRequest(schema) {
  return (req, res, next) => {
    const { error } = schema.validate(req.body);
    if (error) {
      return res.status(400).json({
        error: {
          code: 'VALIDATION_ERROR',
          message: error.message,
          details: error.details
        }
      });
    }
    next();
  };
}
```

### Required Fields

Always validate:
- Required fields exist
- Data types are correct
- Values are within acceptable ranges
- Format matches expected pattern (email, URL, etc.)

## Basic Project Structure

```
api/
├── routes/
│   ├── index.js           # Route registration
│   └── resources.js       # Resource endpoints
├── controllers/
│   └── resourceController.js  # Business logic
├── models/
│   └── resource.js        # Data models
├── middleware/
│   ├── auth.js            # Authentication
│   ├── validate.js        # Validation
│   └── errorHandler.js    # Error handling
└── tests/
    └── resources.test.js  # API tests
```

## Error Handling Pattern

```javascript
// Centralized error handler
app.use((err, req, res, next) => {
  console.error(err.stack);

  const status = err.status || 500;
  const message = err.message || 'Internal server error';

  res.status(status).json({
    error: {
      code: err.code || 'INTERNAL_ERROR',
      message: message
    }
  });
});
```

## Quick Start Checklist

- [ ] Define resource models
- [ ] Set up routes with proper HTTP methods
- [ ] Add request validation
- [ ] Implement error handling
- [ ] Write API tests
- [ ] Document endpoints (OpenAPI/Swagger)

## Testing APIs

```bash
# Test with curl
curl -X GET http://localhost:3000/api/resources
curl -X POST http://localhost:3000/api/resources \
  -H "Content-Type: application/json" \
  -d '{"name":"test"}'
```

## Remember

- This is a basic template, not production-ready
- Add authentication/authorization as needed
- Use validation libraries (Joi, Zod, etc.)
- Log all errors with context
- Version your API (/api/v1/resources)
- Document with OpenAPI/Swagger
\`\`\`

**Step 5: Verify plugin structure**

Run: `ls -R plugins/api-dev/`

Expected output:
```
plugins/api-dev/:
commands/  plugin.json  skills/

plugins/api-dev/commands:
api.md

plugins/api-dev/skills:
SKILL.md
```

**Step 6: Commit api-dev plugin**

```bash
git add plugins/api-dev/
git commit -m "feat: add api-dev plugin with /api command"
```

---

## Task 4: Create Refactoring Plugin

**Files:**
- Create: `plugins/refactoring/plugin.json`
- Create: `plugins/refactoring/commands/refactor.md`
- Create: `plugins/refactoring/skills/SKILL.md`

**Step 1: Create plugin directory structure**

Run: `mkdir -p plugins/refactoring/commands plugins/refactoring/skills`

**Step 2: Create plugin metadata**

Create `plugins/refactoring/plugin.json`:

```json
{
  "name": "refactoring",
  "version": "1.0.0",
  "description": "Safe refactoring workflow",
  "author": "Your Team",
  "commands": ["refactor"],
  "skills": ["refactoring"],
  "autoEnable": false,
  "resources": {
    "commands": {
      "refactor": "commands/refactor.md"
    },
    "skills": {
      "refactoring": "skills/SKILL.md"
    }
  }
}
```

**Step 3: Create slash command**

Create `plugins/refactoring/commands/refactor.md`:

```markdown
@skills/refactoring/SKILL.md
```

**Step 4: Create skill**

Create `plugins/refactoring/skills/SKILL.md`:

```markdown
---
name: Safe Refactoring Workflow
description: Test-driven refactoring with small incremental changes
when_to_use: when improving code quality, removing duplication, or simplifying complex code
version: 1.0.0
---

# Safe Refactoring Workflow

## Overview

Systematic approach to refactoring that maintains functionality through tests and incremental changes.

**Core principle:** Small steps, frequent verification, never break working code.

## The Process

### Phase 1: Identify the Problem

**Common code smells:**
- Long functions (>20 lines)
- Duplicated code
- Complex conditionals (nested if/else)
- Unclear variable names
- Mixed abstraction levels
- Large classes with many responsibilities

**Before refactoring, ask:**
- What makes this code hard to understand?
- What would make it easier to change?
- Is this worth the effort?

### Phase 2: Ensure Test Coverage

**Before changing code:**

1. Check if tests exist
2. Run tests to verify they pass
3. If tests missing, write them first

```bash
# Run tests
npm test
# or pytest
# or go test ./...
```

**Why:** Tests are your safety net. No tests = no confidence.

### Phase 3: Make Small Changes

**Refactor in tiny steps:**

1. Make ONE small change
2. Run tests
3. If tests pass → commit
4. If tests fail → revert, try smaller step
5. Repeat

**Small change examples:**
- Rename one variable
- Extract one function
- Simplify one conditional
- Remove one piece of duplication

### Phase 4: Verify and Commit

After each change:

```bash
# Run tests
npm test

# If passing, commit
git add .
git commit -m "refactor: extract validation logic"
```

**Never commit broken tests.**

## Common Refactoring Patterns

### Extract Function

**Before:**
```javascript
function processOrder(order) {
  // Validate order
  if (!order.items || order.items.length === 0) {
    throw new Error("Order must have items");
  }
  if (!order.customer) {
    throw new Error("Order must have customer");
  }

  // Calculate total
  let total = 0;
  for (const item of order.items) {
    total += item.price * item.quantity;
  }

  // Apply discount
  if (order.customer.isPremium) {
    total *= 0.9;
  }

  return total;
}
```

**After:**
```javascript
function processOrder(order) {
  validateOrder(order);
  const subtotal = calculateSubtotal(order.items);
  return applyDiscount(subtotal, order.customer);
}

function validateOrder(order) {
  if (!order.items || order.items.length === 0) {
    throw new Error("Order must have items");
  }
  if (!order.customer) {
    throw new Error("Order must have customer");
  }
}

function calculateSubtotal(items) {
  return items.reduce((sum, item) => {
    return sum + (item.price * item.quantity);
  }, 0);
}

function applyDiscount(amount, customer) {
  return customer.isPremium ? amount * 0.9 : amount;
}
```

### Rename for Clarity

**Before:**
```python
def calc(d):
    return d * 0.85
```

**After:**
```python
def calculate_discounted_price(original_price):
    DISCOUNT_RATE = 0.85
    return original_price * DISCOUNT_RATE
```

### Simplify Conditionals

**Before:**
```javascript
if (user.age >= 18) {
  if (user.hasLicense) {
    if (user.passedTest) {
      return true;
    }
  }
}
return false;
```

**After:**
```javascript
function canDrive(user) {
  return user.age >= 18
    && user.hasLicense
    && user.passedTest;
}
```

## Refactoring Workflow Checklist

- [ ] Identify code smell or improvement opportunity
- [ ] Ensure tests exist and pass
- [ ] Make one small change
- [ ] Run tests
- [ ] Commit if tests pass
- [ ] Repeat until satisfied

## When to Stop

**Stop refactoring when:**
- Code is clear and maintainable
- No obvious improvements remain
- Time budget exhausted
- Tests are getting harder to maintain

**Don't over-refactor.** Working code > perfect code.

## Remember

- Small steps always beat big rewrites
- Tests must pass after every change
- Commit frequently
- Refactoring changes structure, not behavior
- If tests break, you changed behavior (revert!)
- Clear code > clever code
\`\`\`

**Step 5: Verify plugin structure**

Run: `ls -R plugins/refactoring/`

Expected output:
```
plugins/refactoring/:
commands/  plugin.json  skills/

plugins/refactoring/commands:
refactor.md

plugins/refactoring/skills:
SKILL.md
```

**Step 6: Commit refactoring plugin**

```bash
git add plugins/refactoring/
git commit -m "feat: add refactoring plugin with /refactor command"
```

---

## Task 5: Final Verification and Documentation

**Files:**
- Verify: All files created correctly
- Test: Directory structure matches specification

**Step 1: Verify complete directory structure**

Run: `tree -L 4` (or `find . -type f` if tree not available)

Expected structure:
```
.
├── .gitignore
├── README.md
├── manifest.json
├── docs/
│   └── plans/
│       └── 2025-10-15-claude-code-marketplace.md
└── plugins/
    ├── api-dev/
    │   ├── commands/
    │   │   └── api.md
    │   ├── plugin.json
    │   └── skills/
    │       └── SKILL.md
    ├── code-review/
    │   ├── commands/
    │   │   └── review.md
    │   ├── plugin.json
    │   └── skills/
    │       └── SKILL.md
    └── refactoring/
        ├── commands/
        │   └── refactor.md
        ├── plugin.json
        └── skills/
            └── SKILL.md
```

**Step 2: Verify JSON files are valid**

Run validation for each JSON file:

```bash
# Check manifest
python3 -m json.tool manifest.json > /dev/null && echo "manifest.json: valid"

# Check plugin configs
python3 -m json.tool plugins/code-review/plugin.json > /dev/null && echo "code-review/plugin.json: valid"
python3 -m json.tool plugins/api-dev/plugin.json > /dev/null && echo "api-dev/plugin.json: valid"
python3 -m json.tool plugins/refactoring/plugin.json > /dev/null && echo "refactoring/plugin.json: valid"
```

Expected: All files should show "valid"

**Step 3: Verify markdown files exist and have content**

Run: `wc -l plugins/*/commands/*.md plugins/*/skills/SKILL.md`

Expected: All files should have line counts > 0

**Step 4: Create final summary in README**

Verify README.md contains:
- Installation instructions
- List of all 3 plugins
- Usage examples for each command
- Learning objectives

**Step 5: Final commit**

```bash
git add -A
git commit -m "docs: finalize marketplace documentation and verification"
```

**Step 6: Display completion summary**

Output the following summary:

```
✓ Marketplace structure complete

Plugins created:
  1. code-review (/review command)
  2. api-dev (/api command)
  3. refactoring (/refactor command)

Next steps:
  1. Push to private git repository
  2. Install marketplace: /plugin marketplace add <git-url>
  3. Install individual plugins: /plugin add <plugin-name>
  4. Use slash commands: /review, /api, /refactor

All files validated and committed.
```

---

## Execution Complete

This plan creates a complete Claude Code marketplace demonstration with:
- 3 independent plugins (code-review, api-dev, refactoring)
- Each plugin: slash command + skill
- Proper JSON manifests and configuration
- Documentation for installation and usage
- Git-ready structure for private repository hosting

The implementation follows:
- @skills/testing/test-driven-development (verification at each step)
- DRY principle (consistent plugin structure)
- YAGNI principle (minimal viable demonstration)
- Frequent commits (after each plugin completion)
