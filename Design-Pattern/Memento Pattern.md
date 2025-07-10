
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

### 📝 Use Case: Text Editor with Undo Feature
We want to:
- Allow the user to type into a text editor.
- Save the current state.
- Undo changes and return to the previous state.
But we want to hide the internal structure of the Editor class (e.g., formatting rules, internal data structures).

### ✅ Memento Pattern C# Implementation

```
using System;
using System.Collections.Generic;

#region Memento
// Memento: Captures internal state
class EditorMemento
{
    public string Content { get; }

    public EditorMemento(string content)
    {
        Content = content;
    }
}
#endregion

#region Originator
// Originator: The main object whose state we want to save/restore
class Editor
{
    private string _content = "";

    public void Type(string words)
    {
        _content += words;
    }

    public void ShowContent()
    {
        Console.WriteLine("Editor Content: " + _content);
    }

    public EditorMemento Save()
    {
        return new EditorMemento(_content);
    }

    public void Restore(EditorMemento memento)
    {
        _content = memento.Content;
    }
}
#endregion

#region Caretaker
// Caretaker: Manages the history of mementos
class History
{
    private Stack<EditorMemento> _history = new Stack<EditorMemento>();

    public void Backup(EditorMemento memento)
    {
        _history.Push(memento);
    }

    public EditorMemento Undo()
    {
        if (_history.Count > 0)
            return _history.Pop();
        return null;
    }
}
#endregion

#region Demo
// Demo
class Program
{
    static void Main()
    {
        var editor = new Editor();
        var history = new History();

        editor.Type("Hello ");
        history.Backup(editor.Save());  // Save point 1

        editor.Type("World!");
        history.Backup(editor.Save());  // Save point 2

        editor.Type(" This is final.");
        editor.ShowContent(); // Output: Hello World! This is final.

        Console.WriteLine("\n-- Undo once --");
        editor.Restore(history.Undo());
        editor.ShowContent(); // Output: Hello World!

        Console.WriteLine("\n-- Undo twice --");
        editor.Restore(history.Undo());
        editor.ShowContent(); // Output: Hello
    }
}
#endregion

```
