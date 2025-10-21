#  Go Language Learning Notes

---

##  Why Choose Go (Golang)

**Question:** Why choose Go? What makes Go unique, and where does it thrive?

**Answer (summary):**
- Go is made by Google engineers for **speed**, **simplicity**, and **concurrency**.
- It combines:
  - Speed of **C**
  - Simplicity of **Python**
  - Power for **servers & cloud**
- Go’s build system is fast due to **no circular dependencies** and **package caching**.
- **Compiled to machine code** → instant startup and great performance.
- **Concurrency model (goroutines)** makes it perfect for scalable systems.
- **Static typing** catches errors early.
- **Garbage collector** handles memory automatically.

**Thrives in:**
- Cloud computing
- Backend web services
- Networking tools
- DevOps tools (Docker, Kubernetes)
- CLI tools

---

##  Drawbacks of Go (and When Not to Use It)

**Question:** What are Go’s drawbacks? When should we not use it?

**Answer (summary):**
- Limited OOP features (no inheritance).
- Verbose error handling (`if err != nil` everywhere).
- Large binary sizes.
- GC pauses can affect real-time systems.
- Not ideal for GUI, AI/ML, or quick scripting.

**Avoid Go for:**
- Front-end or UI-heavy apps
- AI / Data science
- Real-time gaming or hardware drivers
- Projects requiring complex OOP hierarchy

 **Best for:** servers, APIs, concurrent systems, and cloud tools.

---

##  Context in Go (`context.Context`)

**Question:** What is `context.Context`? Do we make it ourselves or get it from the package?

**Answer:**
- `context.Context` is an **interface** from the **`context`** package.
- You **don’t implement it**; you **get it** using helper functions:
  - `context.Background()`
  - `context.WithCancel(parent)`
  - `context.WithTimeout(parent, d)`
  - `context.WithDeadline(parent, t)`
  - `context.WithValue(parent, key, value)`
- It helps:
  - **Cancel requests**
  - **Handle timeouts**
  - **Avoid hanging goroutines or DB calls**
- Always `defer cancel()` when you create a context.
- It’s **cooperative** — it **signals** goroutines to stop, doesn’t force them.

---

##  Channels — Unbuffered vs Buffered

**Question:** What are unbuffered and buffered channels? When does deadlock occur?

**Answer:**

| Type | Description | Use Case |
|------|--------------|----------|
| **Unbuffered** | No storage; send blocks until receiver is ready. | Synchronization & signaling. |
| **Buffered** | Has queue (buffer); sender doesn’t block until full. | Asynchronous communication, pipelines. |

**Deadlock with unbuffered:** if send or receive happens without a matching partner.  
**Deadlock with buffered:** if buffer is full or empty forever.

---

##  Deadlocks — When They Happen

**Question:** What are deadlock conditions in Go?

**Answer:**
Deadlocks happen when **all goroutines are waiting**, and none can proceed.

**Common causes:**
1. Sending with no receiver.
2. Receiving with no sender.
3. Full buffered channel with no receiver.
4. Empty buffered channel with no sender.
5. Two goroutines waiting on each other.
6. Forgetting to close a channel used in `for range`.
7. `select` with no ready case and no `default`.

---

##  Difference Between `var`, `:=`, `new()`, and `make()`

| Keyword | Works For | Returns | Initializes | Example |
|----------|------------|----------|--------------|----------|
| `var` | All types | Value | Zero value | `var x int` |
| `:=` | All types (inside funcs) | Value | Given value | `x := 10` |
| `new()` | Value types (int, struct) | Pointer (`*T`) | Zero value | `p := new(int)` |
| `make()` | Slices, maps, channels | Ready-to-use value (not pointer) | Internal data structure | `m := make(map[string]int)` |

**Mental model:**
- `var` → just declare  
- `:=` → short declare inside func  
- `new()` → get pointer to blank value  
- `make()` → create and initialize built-in reference type  

---

##  Inheritance vs Struct Embedding

**Question:** What’s the difference?

| Concept | Inheritance (OOP) | Struct Embedding (Go) |
|----------|--------------------|------------------------|
| Relationship | “is-a” | “has-a” |
| Mechanism | Class hierarchy | Composition |
| Overriding | Yes | No (only shadows) |
| Type hierarchy | Exists | None |
| Access control | Protected/public | Exported/unexported |
| Polymorphism | Built-in | Done via interfaces |

**Go way:**  
> Instead of “Dog is an Animal,” Go says “Dog has an Animal.”

**Shadowing example:**
```go
type Animal struct{}
func (Animal) Speak() { fmt.Println("Animal sound") }

type Dog struct{ Animal }
func (Dog) Speak() { fmt.Println("Dog barks") }

d := Dog{}
d.Speak()         // Dog barks
d.Animal.Speak()  // Animal sound
```

---

##  Polymorphism in Go

**Question:** How can we do polymorphism in Go?

**Answer:**
Through **interfaces**, not inheritance.

### Example:
```go
type Animal interface {
    Speak()
}

type Dog struct{}
type Cat struct{}

func (Dog) Speak() { fmt.Println("Woof!") }
func (Cat) Speak() { fmt.Println("Meow!") }

func MakeItSpeak(a Animal) {
    a.Speak()
}
```

 Go’s polymorphism =  
“If it has the required method, it satisfies the interface.”

**Key points:**
- No `implements` keyword.
- Interface variables can store any type implementing its methods.
- Real polymorphism achieved via **behavior**, not class hierarchy.

---

##  Struct Embedding and Methods

**Your doubt:**  
When embedding, can we override methods or reuse them for other types?

**Answer:**
- Embedding copies methods and fields from inner struct.
- You can **shadow** (not override) methods by redeclaring with the same name.
- You can **still access the inner method** explicitly.
- Each struct can have methods with same names but different receivers (that’s not overriding — it’s independent).

 For real polymorphism, Go uses **interfaces**, not struct embedding.

---
