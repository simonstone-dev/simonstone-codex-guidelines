# Examples

Short examples showing how the four principles change Codex behavior.

## 1. Think Before Coding

### Hidden assumptions

**User request:** "Add user export."

**Common failure:** implement a full export system that assumes all users, CSV and JSON, specific fields, and a file destination.

**Better Codex behavior:**

```text
Before implementing, I need to clarify:

1. Scope: all users or a filtered subset?
2. Output: API response, browser download, or background job?
3. Fields: which fields are safe to export?
4. Volume: how many users should this handle?

Simplest likely path: add a paginated JSON endpoint unless file download is required.
```

## 2. Simplicity First

### Premature abstraction

**User request:** "Add a discount calculation."

**Overcomplicated version:** strategy interfaces, config classes, multiple discount backends, and extension points.

**Simpler version:**

```python
def calculate_discount(amount: float, percent: float) -> float:
    return amount * (percent / 100)
```

Add complexity only when the requirement actually needs multiple discount types.

## 3. Surgical Changes

### Drive-by refactor

**User request:** "Fix empty email crashing the validator."

**Common failure:** rewrite the validator, change comments, add username rules, and reformat unrelated lines.

**Better behavior:** change only the lines that handle empty email and leave unrelated validation alone.

```diff
 def validate_user(user_data):
-    if not user_data.get('email'):
+    email = user_data.get('email', '')
+    if not email or not email.strip():
         raise ValueError('Email required')
 
-    if '@' not in user_data['email']:
+    if '@' not in email:
         raise ValueError('Invalid email')
```

## 4. Goal-Driven Execution

### Vague task to verifiable goal

**User request:** "Fix sorting when there are duplicate scores."

**Better Codex behavior:**

```text
Plan:
1. Add or identify a test with duplicate scores -> verify it reproduces the unstable behavior.
2. Add deterministic tie-breaking -> verify the duplicate-score test passes.
3. Run existing sorting tests -> verify no regression.
```

The important change is not only the code. It is the verification loop.

## Anti-Patterns Summary

| Principle | Anti-pattern | Better behavior |
| --- | --- | --- |
| Think Before Coding | Silently chooses one interpretation | State assumptions and ask when needed |
| Simplicity First | Adds abstractions for one use case | Use the direct implementation first |
| Surgical Changes | Rewrites nearby code while fixing a bug | Change only the bug-related lines |
| Goal-Driven Execution | Ends with "should work" | Run or describe concrete checks |
