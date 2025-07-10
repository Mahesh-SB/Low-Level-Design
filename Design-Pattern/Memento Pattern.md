
# Memento : Creational Pattern
The Memento Pattern solves the problem of saving and restoring the internal state of an object without violating its encapsulation.

## 🔧 Problem It Solves
You want to:
- Capture an object's state at a certain point in time.
- Restore that state later (undo/rollback).
- But you don't want to expose the internal structure or variables of the object to the outside world.

## ✅ When to Use
- Implementing undo/redo in editors, drawing apps, IDEs, etc.
- Keeping snapshots of objects for recovery (e.g., game checkpoints).
- Handling rollback operations in transactions.

## ✅ “Without violating its encapsulation” means ?
🔐 Encapsulation = Keeping data safe and hidden inside an object. You do not expose or allow access to the private/internal data of an object from the outside.

### ❌ What violating encapsulation looks like:
Suppose you have a class Document with a private field:
```
class Document {
    private string content;

    public string GetContent() {
        return content;  // Exposes internal state
    }

    public void SetContent(string c) {
        content = c;     // Allows external modification
    }
}
```
Now, any external code can read and change the content directly using GetContent() or SetContent(). This breaks encapsulation because the internal data is no longer protected.
