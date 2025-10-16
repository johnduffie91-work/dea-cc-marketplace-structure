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
