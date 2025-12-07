# Code Quality

*Reference for **[technical-implementation](../SKILL.md)***

---

Apply standard quality principles. Defer to project-specific skills for conventions.

## Key Rules

### DRY: Rule of Three

Don't abstract until you have three instances. Two similar blocks might diverge.

```python
# Two instances? Keep them separate
# Three instances? Extract

def cache_key(user_id, *parts):
    return f"cache:user:{user_id}" + ((":" + ":".join(parts)) if parts else "")
```

### YAGNI: Only What's in the Plan

Before adding anything, ask: "Is this in the plan?"

```python
# Bad: Building for imaginary future
class Cache:
    def get(self, key): ...
    def set(self, key, value): ...
    def get_multi(self, keys): ...      # Not in plan
    def set_multi(self, items): ...     # Not in plan

# Good: What the plan requires
class Cache:
    def get(self, key): ...
    def set(self, key, value): ...
```

**Common violations**:
- Parameters "in case we need them"
- Abstract base classes for single implementations
- Configuration for hardcoded values
- Generic solutions for specific problems

### Complexity: Keep It Low

**Warning signs**:
- 3+ levels of nesting
- Functions over 30 lines
- Many branches in one function

**Fix with early returns**:
```python
# Before
def process(order):
    if order.is_valid():
        if order.has_stock():
            if order.payment_cleared():
                # deep nesting

# After
def process(order):
    if not order.is_valid():
        return Error("Invalid")
    if not order.has_stock():
        return Error("No stock")
    if not order.payment_cleared():
        return Error("Payment failed")
    return complete(order)
```

### Testability: Inject Dependencies

```python
# Hard to test
class OrderService:
    def create(self, data):
        db = Database()  # Can't mock
        db.save(Order(data))

# Easy to test
class OrderService:
    def __init__(self, db: Database):
        self.db = db

    def create(self, data):
        self.db.save(Order(data))
```

## Anti-Patterns

| Pattern | Fix |
|---------|-----|
| God class | Break into focused components |
| Magic numbers | Use named constants |
| Deep nesting (3+) | Early returns, extract methods |
| Long parameter lists (4+) | Use data object |
| Boolean parameter | Two separate methods |

## Project Conventions

Check `.claude/skills/` for:
- Framework patterns
- Code style
- Architecture conventions

**When project conventions conflict with these principles, follow project conventions.**
