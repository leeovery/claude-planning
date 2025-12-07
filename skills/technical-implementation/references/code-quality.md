# Code Quality

*Part of **[technical-implementation](../SKILL.md)** | See also: **[tdd-workflow.md](tdd-workflow.md)** · **[plan-execution.md](plan-execution.md)***

---

General quality principles for implementation. Defer to project-specific skills for framework conventions.

## DRY: Don't Repeat Yourself

### The Principle

Every piece of knowledge should have a single, authoritative representation.

### Good DRY

Repeated logic extracted:
```php
// Before: Logic duplicated
function getUserCacheKey(int $userId): string {
    return "cache:user:{$userId}";
}
function getMetricsCacheKey(int $userId): string {
    return "cache:user:{$userId}:metrics";
}

// After: Single source of truth
function cacheKey(int $userId, string ...$parts): string {
    return "cache:user:{$userId}" . ($parts ? ':' . implode(':', $parts) : '');
}
```

### Bad DRY: Premature Abstraction

Don't create abstractions for code used once or twice:
```php
// Over-engineered: Used in one place
class CacheKeyBuilder {
    public function __construct(private string $prefix) {}
    public function build(string ...$parts): string {
        return implode(':', [$this->prefix, ...$parts]);
    }
}

// Better: Just write the string
$cacheKey = "cache:user:{$userId}:metrics";
```

### Rule of Three

Wait until you have three instances before abstracting. Two similar blocks might diverge. Three suggests a pattern.

## SOLID Principles

### Single Responsibility

Each class/function does one thing.

```php
// Bad: Does too much
class UserService {
    public function createUser($data) { }
    public function sendWelcomeEmail($user) { }
    public function generateInvoice($user) { }
    public function exportToCsv($users) { }
}

// Good: Focused responsibility
class UserService {
    public function create($data) { }
    public function update($user, $data) { }
    public function delete($user) { }
}
```

### Open/Closed

Open for extension, closed for modification.

```php
// Bad: Modify class to add new behavior
class PaymentProcessor {
    public function process(Payment $payment) {
        match ($payment->type) {
            'credit' => $this->processCredit($payment),
            'paypal' => $this->processPaypal($payment),
            // Add new case for each type...
        };
    }
}

// Good: Extend without modifying
class PaymentProcessor {
    public function __construct(private array $handlers) {}

    public function process(Payment $payment) {
        $this->handlers[$payment->type]->process($payment);
    }
}
```

### Liskov Substitution

Subtypes must be substitutable for their base types.

```php
// Bad: Square breaks Rectangle's contract
class Rectangle {
    public function setWidth(int $w) { $this->width = $w; }
    public function setHeight(int $h) { $this->height = $h; }
}
class Square extends Rectangle {
    public function setWidth(int $w) {
        $this->width = $this->height = $w; // Violates expectations
    }
}

// Good: Separate types
interface Shape { public function area(): float; }
class Rectangle implements Shape { }
class Square implements Shape { }
```

### Interface Segregation

Don't force classes to implement methods they don't use.

```php
// Bad: Forces empty implementations
interface Worker {
    public function work();
    public function eat();
    public function sleep();
}
class Robot implements Worker {
    public function eat() { } // Robots don't eat
    public function sleep() { } // Robots don't sleep
}

// Good: Separate interfaces
interface Workable { public function work(); }
interface Eatable { public function eat(); }

class Robot implements Workable { }
class Human implements Workable, Eatable { }
```

### Dependency Inversion

Depend on abstractions, not concretions.

```php
// Bad: Tightly coupled
class OrderService {
    private MySQLDatabase $db;
    public function __construct() {
        $this->db = new MySQLDatabase(); // Concrete dependency
    }
}

// Good: Depends on abstraction (Laravel style)
class OrderService {
    public function __construct(private Database $db) { }
}
```

## Cyclomatic Complexity

### Keep It Low

Cyclomatic complexity counts independent paths through code. High complexity = hard to test and maintain.

### Warning Signs

- Deeply nested conditionals (3+ levels)
- Many branches in one function
- Long switch/match statements
- Functions over 20-30 lines

### Reducing Complexity

**Extract methods:**
```php
// Before: Complex nested logic
public function processOrder(Order $order) {
    if ($order->isValid()) {
        if ($order->hasStock()) {
            if ($order->paymentCleared()) {
                // ... more nesting

// After: Flat and clear
public function processOrder(Order $order) {
    if (!$order->isValid()) return new Error('Invalid order');
    if (!$order->hasStock()) return new Error('Out of stock');
    if (!$order->paymentCleared()) return new Error('Payment failed');
    return $this->completeOrder($order);
}
```

**Early returns:**
```php
// Before: Single exit point
public function getDiscount(User $user): int {
    $discount = 0;
    if ($user->is_premium) {
        if ($user->years > 5) {
            $discount = 20;
        } else {
            $discount = 10;
        }
    }
    return $discount;
}

// After: Early returns
public function getDiscount(User $user): int {
    if (!$user->is_premium) return 0;
    if ($user->years > 5) return 20;
    return 10;
}
```

**Replace conditionals with polymorphism:**
When you have many conditionals based on type, consider polymorphism instead.

## YAGNI: You Aren't Gonna Need It

### Don't Build Speculatively

Only implement what's required now:

```python
# Bad: Building for imaginary future
class Cache:
    def get(self, key): ...
    def set(self, key, value): ...
    def get_multi(self, keys): ...      # Not needed yet
    def set_multi(self, items): ...     # Not needed yet
    def get_with_fallback(self, ...): ... # Not needed yet
    def invalidate_pattern(self, ...): ... # Not needed yet

# Good: What the plan requires
class Cache:
    def get(self, key): ...
    def set(self, key, value): ...
    def invalidate(self, key): ...
```

### Common YAGNI Violations

- Adding parameters "in case we need them"
- Creating abstract base classes for single implementations
- Adding configuration for hardcoded values
- Building plugin systems when one implementation exists
- Adding caching before measuring performance
- Creating generic solutions for specific problems

### The Test

Before adding anything, ask: "Is this in the plan?" If no, don't add it.

## Readability

### Naming

Names should reveal intent:

```python
# Bad
d = 86400
def calc(u, t):
    return u.b * t

# Good
SECONDS_PER_DAY = 86400
def calculate_balance(user, days):
    return user.balance * days
```

### Function Length

Functions should do one thing. If you need comments to separate sections, extract methods:

```python
# Bad: Multiple responsibilities
def process_order(order):
    # Validate order
    if not order.items:
        raise ValueError("Empty order")
    # ... 10 more lines of validation

    # Calculate totals
    subtotal = sum(item.price for item in order.items)
    # ... 10 more lines of calculation

    # Save to database
    db.save(order)
    # ... more database logic

# Good: Single responsibility each
def process_order(order):
    validate_order(order)
    calculate_totals(order)
    save_order(order)
```

### Comments

Code should be self-documenting. Comments explain WHY, not WHAT:

```python
# Bad: Explains what (obvious from code)
# Increment counter by 1
counter += 1

# Good: Explains why (not obvious)
# Redis INCR is atomic, prevents race conditions
counter = redis.incr("requests")
```

### Avoid Clever Code

```python
# Clever but obscure
result = data and data.get('key') or default

# Clear
result = data.get('key', default) if data else default
```

## Testability

### Dependency Injection

Pass dependencies in, don't create them inside:

```python
# Hard to test
class OrderService:
    def create(self, data):
        db = Database()  # Can't mock this
        db.save(Order(data))

# Easy to test
class OrderService:
    def __init__(self, db: Database):
        self.db = db

    def create(self, data):
        self.db.save(Order(data))
```

### Pure Functions Where Possible

Functions that don't modify state or depend on external state are easy to test:

```python
# Impure: Depends on global state
def calculate_tax(amount):
    return amount * config.TAX_RATE

# Pure: All inputs explicit
def calculate_tax(amount, tax_rate):
    return amount * tax_rate
```

### Avoid Hidden Dependencies

```python
# Hidden dependency on datetime.now()
def is_expired(token):
    return token.expires_at < datetime.now()

# Explicit: Can test with any time
def is_expired(token, current_time):
    return token.expires_at < current_time
```

## Anti-Patterns to Avoid

### God Classes

Classes that do everything. Break into focused components.

### Magic Numbers/Strings

Use named constants:
```python
# Bad
if response.status == 200:
    cache.set(key, value, 3600)

# Good
HTTP_OK = 200
ONE_HOUR = 3600
if response.status == HTTP_OK:
    cache.set(key, value, ONE_HOUR)
```

### Deep Nesting

Maximum 2-3 levels. Use early returns, extract methods.

### Long Parameter Lists

More than 3-4 parameters suggests the function does too much or needs a data object:
```python
# Bad
def create_user(name, email, age, address, phone, company, role, ...):

# Good
def create_user(user_data: UserData):
```

### Boolean Parameters

Often a sign of a function doing two things:
```python
# Bad
def get_users(include_inactive=False):

# Good
def get_active_users():
def get_all_users():
```

## Project-Specific Standards

This document covers general principles. For framework-specific patterns:

- Check `.claude/skills/` for project skills
- Follow established patterns in the codebase
- Defer to project conventions when they conflict with general principles
