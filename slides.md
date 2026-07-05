---
theme: default
title: 'Design Patterns: Retire, Retain, Replace'
titleTemplate: '%s · C# & Wolverine'
highlighter: shiki
lineNumbers: false
transition: fade-out
layout: cover
background: '#0F1117'
class: 'text-left'
fonts:
  sans: 'Inter'
  mono: 'JetBrains Mono'
---

<div class="font-mono text-xs tracking-widest uppercase text-amber-500 mb-5">
  // C# · Wolverine · Marten
</div>

# Design Patterns:

# **Retire, Retain, Replace**

<p class="text-slate-400 mt-3 text-lg leading-relaxed">
  Which patterns have aged out, which remain essential,<br>
  and what Wolverine absorbs from your codebase
</p>

<style>
  h1 { color: #E8E9F0 !important; border: none !important; letter-spacing: -0.03em; line-height: 1.15 !important; }
</style>

---
layout: section
background: '#1A1F2E'
class: 'text-white'
---

<div class="font-mono text-sm tracking-widest uppercase text-amber-500 mb-3">Prologue</div>

# The Dark Days of the 90s

_Why the Gang of Four and SOLID were born_

---

## The World Before Patterns

<v-clicks>

**The hardware:** 486 processors. 8–16 MB RAM. Hard drives measured in megabytes.

**The languages:** C++ ruled. No generics. No lambdas. No garbage collection. Header files that caused 45-minute recompiles.

**The process:** Waterfall. 18-month release cycles. QA gates that lasted weeks.

**The deployment:** Floppy disks. CD-ROMs. Physical servers you had to drive to.

</v-clicks>

<v-click>

<div class="mt-6 bg-red-50 border-l-4 border-red-500 rounded-r px-5 py-4 text-base text-red-900">
  Every change was expensive.<br>
  Every mistake was costly.<br>
  <strong>Wrong code could kill a release.</strong>
</div>

</v-click>

---

## The Compilation Wall

In 1994, changing a single header file could trigger:

<v-clicks>

- Full project recompilation — **45 minutes to 2 hours**
- The entire team blocked waiting for the build
- No background compilation. No incremental builds.
- A coffee break became a lunch break became a "go home early"

</v-clicks>

<v-click>

<div class="mt-6 bg-slate-800 text-slate-200 rounded-lg px-5 py-4 font-mono text-sm">
"The build server was a shared resource.<br>
You queued your build. You waited your turn.<br>
You prayed it compiled the first time."<br>
<span class="text-slate-400 text-xs mt-2 block">— Veteran developer, recalling Visual C++ 4.0</span>
</div>

</v-click>

---

## The Deployment Ice Age

| What we have now          | What they had then                              |
| ------------------------- | ----------------------------------------------- |
| `git push` → deployed     | Shelf the binaries → QA → staging → production  |
| Rollback in seconds       | Reinstall from CD-ROM                           |
| Feature flags             | Recompile, repackage, redistribute             |
| Canary deployments        | Big bang releases to the entire user base       |
| Hotfixes in minutes       | Emergency patch → burn new discs → overnight it |

<v-click>

<div class="mt-6 bg-amber-50 border-l-4 border-amber-500 rounded-r px-4 py-3 text-sm text-amber-900">
  <strong>The forcing function:</strong> When deployment costs weeks,<br>
  you build systems that absorb change without redeploying.
</div>

</v-click>

---

## Why Patterns Emerged: Survival, Not Elegance

The Gang of Four weren't academics inventing abstractions.

They were **practitioners documenting what worked** under constraints we've forgotten:

<v-clicks>

- **Open/Closed Principle** — because changing existing code risked breaking it, and recompiling took hours
- **Strategy Pattern** — because C++ had no lambdas, and `function pointers` were unsafe
- **Builder Pattern** — because constructors couldn't have named parameters, and 12-arg constructors were unreadable
- **Singleton** — because global state was the only way to share resources without passing everything down the call stack

</v-clicks>

<v-click>

<div class="mt-5 bg-slate-100 rounded-lg px-4 py-3 text-sm text-slate-600 italic">
  These weren't theoretical ideals.<br>
  They were <strong>survival mechanisms</strong> for hostile environments.
</div>

</v-click>

---

## SOLID's Hidden Assumption

Robert Martin formulated SOLID between 1995–2000.

Every principle addressed _one specific, painful cost_:

| Principle | The pain it addressed                                 |
| --------- | ----------------------------------------------------- |
| **SRP**   | Classes that do too much break too often              |
| **OCP**   | Modify code → recompile everything → blocked team     |
| **LSP**   | Inheritance hierarchies that broke at runtime         |
| **ISP**   | Fat interfaces forced recompilation of unrelated code|
| **DIP**   | Concrete dependencies meant hardcoded, inflexible code|

<v-click>

<div class="mt-5 bg-red-50 border-l-4 border-red-400 rounded-r px-4 py-3 text-sm text-red-800">
  SOLID assumed: <strong>change is expensive.</strong><br>
  When change becomes cheap, the calculus shifts.
</div>

</v-click>

---

## The Question That Changes Everything

<div class="bg-slate-800 text-slate-100 rounded-lg px-8 py-6 my-6 text-center">
<p class="text-2xl font-light leading-relaxed mb-4">
  What if recompilation took 2 seconds,<br>
  not 2 hours?
</p>
<p class="text-slate-400 text-lg">
  What if deployment was `git push`?<br>
  What if you could change production in minutes?
</p>
</div>

<v-clicks>

- Would you still design for _extensibility_ over _simplicity_?
- Would you still build abstraction layers to avoid touching code?
- Would you still pay the indirection tax for problems that vanished?

</v-clicks>

<v-click>

<div class="mt-5 bg-amber-50 border-l-4 border-amber-500 rounded-r px-4 py-3 text-amber-900">
  The patterns didn't become wrong.<br>
  <strong>The constraints that made them necessary — disappeared.</strong>
</div>

</v-click>

---

## The Diagnostic Question

Before evaluating any pattern, ask one question:

<div class="bg-amber-50 border-l-4 border-amber-500 rounded-r-lg px-5 py-4 my-6 text-xl italic text-amber-900">
  "What specific problem did this solve — and does that problem still exist?"
</div>

Patterns don't become _wrong_. Their **forcing functions** disappear.

<v-clicks>

- Slow compilation → language workarounds become obsolete
- Expensive deployment → process-safety patterns become obsolete
- Missing language features → structural ceremony becomes obsolete
- Single-machine assumption → a whole new set is still very much needed

</v-clicks>

---

## Three Mechanisms of Expiry

| Mechanism                | What it killed                            | Example                   |
| ------------------------ | ----------------------------------------- | ------------------------- |
| **Language features**    | Patterns compensating for missing syntax  | Strategy → `Func<T,R>`    |
| **Framework absorption** | Patterns the runtime does better          | Singleton → DI container  |
| **Deployment shift**     | Practices tied to expensive change cycles | OCP → redeploy in minutes |

<v-click>

<div class="mt-6 bg-slate-100 rounded-lg px-4 py-3 text-sm text-slate-600">
  The patterns that survive solve <strong>domain</strong> or <strong>structural</strong> problems —
  not language limitations or deployment economics.
</div>

</v-click>

---
layout: section
background: '#1A1F2E'
class: 'text-white'
---

<div class="font-mono text-sm tracking-widest uppercase text-amber-500 mb-3">Part I</div>

# What the Language Absorbed

_GoF patterns as workarounds for C++ constraints_

---

## Patterns the Language Absorbed

| Pattern             | What replaced it                                 |
| ------------------- | ------------------------------------------------ |
| **Strategy**        | `Func<T,R>` / `Action<T>`                        |
| **Visitor**         | `switch` on `sealed` record hierarchies          |
| **Template Method** | Higher-order functions, `Func<T>` parameters     |
| **Builder**         | Named params, `required` members, `with`         |
| **State**           | Pattern matching on discriminated unions         |
| **Null Object**     | `#nullable enable`, `?.`, `??`                   |
| **Memento**         | `record` + `with`-expression                     |
| **Observer**        | `event`, `IObservable<T>`, `IAsyncEnumerable<T>` |
| **Iterator**        | `IEnumerable<T>`, `yield return`                 |
| **Singleton**       | `services.AddSingleton<T>()`                     |

---
layout: two-cols-header
---

## A Concrete Example: Strategy Pattern

The pattern existed to pass _behaviour as a parameter_ in languages without first-class functions.

::left::

<div class="font-mono text-xs tracking-widest uppercase text-slate-400 mb-3">Before — C# 1.0</div>

```csharp
interface ISortStrategy {
    void Sort(List<int> data);
}

class BubbleSort : ISortStrategy {
    public void Sort(List<int> data) { ... }
}

var sorter = new Sorter(new BubbleSort());
sorter.Execute(data);
```

::right::

<div class="font-mono text-xs tracking-widest uppercase text-amber-600 mb-3">After — Modern C#</div>

```csharp
void Sort(
    List<int> data,
    Action<List<int>> strategy)
{
    strategy(data);
}

Sort(data, d => d.Sort());
```

<div class="mt-5 text-sm text-slate-500">
  The pattern wasn't discovered wisdom.<br>
  It was <strong>compensation for a missing feature.</strong>
</div>

---
layout: two-cols-header
---

## The Builder Pattern: Ceremony vs. Syntax

Named and optional parameters eliminated the need for a separate Builder class.

::left::

<div class="font-mono text-xs tracking-widest uppercase text-slate-400 mb-3">Before — 80 lines of ceremony</div>

```csharp
public class HttpRequestBuilder
{
    private string _url;
    private string _method = "GET";
    private Dictionary<string, string> _headers;
    private string _body;
    private TimeSpan _timeout = TimeSpan.FromSeconds(30);

    public HttpRequestBuilder WithUrl(string url)
    {
        _url = url;
        return this;
    }

    public HttpRequestBuilder WithMethod(string method)
    {
        _method = method;
        return this;
    }

    public HttpRequestBuilder WithHeader(string key, string value)
    {
        _headers ??= new();
        _headers[key] = value;
        return this;
    }

    public HttpRequestBuilder WithBody(string body)
    {
        _body = body;
        return this;
    }

    public HttpRequestBuilder WithTimeout(TimeSpan timeout)
    {
        _timeout = timeout;
        return this;
    }

    public HttpRequest Build() => new(_url, _method, _headers, _body, _timeout);
}

// Usage
var request = new HttpRequestBuilder()
    .WithUrl("https://api.example.com")
    .WithMethod("POST")
    .WithHeader("Content-Type", "application/json")
    .WithBody("{\"name\":\"test\"}")
    .WithTimeout(TimeSpan.FromMinutes(1))
    .Build();
```

::right::

<div class="font-mono text-xs tracking-widest uppercase text-amber-600 mb-3">After — constructor with defaults</div>

```csharp
// Type definition (5 lines)
public record HttpRequest(
    string Url,
    string Method = "GET",
    Dictionary<string, string>? Headers = null,
    string? Body = null,
    TimeSpan Timeout = default
);

// Usage
var request = new HttpRequest(
    Url: "https://api.example.com",
    Method: "POST",
    Headers: new()
    {
        ["Content-Type"] = "application/json"
    },
    Body: "{\"name\":\"test\"}",
    Timeout: TimeSpan.FromMinutes(1)
);

// Or with object initializer
var request2 = new HttpRequest("https://api.example.com")
{
    Method = "POST",
    Body = "{\"name\":\"test\"}"
};
```

---
layout: two-cols-header
---

## The Visitor Pattern: Double Dispatch Dissolved

Pattern matching on sealed hierarchies replaced the Visitor's type-safe traversal.

::left::

<div class="font-mono text-xs tracking-widest uppercase text-slate-400 mb-3">Before — 60 lines, 5 types</div>

```csharp
interface IExpressionVisitor
{
    void Visit(Literal literal);
    void Visit(Addition addition);
    void Visit(Multiplication multiplication);
}

abstract class Expression
{
    public abstract void Accept(IExpressionVisitor visitor);
}

class Literal : Expression
{
    public double Value { get; }
    public Literal(double value) => Value = value;
    public override void Accept(IExpressionVisitor v)
        => v.Visit(this);
}

class Addition : Expression
{
    public Expression Left { get; }
    public Expression Right { get; }
    public Addition(Expression left, Expression right)
        => (Left, Right) = (left, right);
    public override void Accept(IExpressionVisitor v)
        => v.Visit(this);
}

class Evaluator : IExpressionVisitor
{
    public double Result { get; private set; }

    public void Visit(Literal literal)
        => Result = literal.Value;

    public void Visit(Addition addition)
    {
        addition.Left.Accept(this);
        var left = Result;
        addition.Right.Accept(this);
        Result = left + Result;
    }
}
```

::right::

<div class="font-mono text-xs tracking-widest uppercase text-amber-600 mb-3">After — 15 lines, pattern matching</div>

```csharp
sealed abstract record Expression;

sealed record Literal(double Value)
    : Expression;

sealed record Addition(Expression Left, Expression Right)
    : Expression;

sealed record Multiplication(Expression Left, Expression Right)
    : Expression;

// Evaluation in one expression
double Evaluate(Expression expr) => expr switch
{
    Literal(var value) => value,
    Addition(var left, var right)
        => Evaluate(left) + Evaluate(right),
    Multiplication(var left, var right)
        => Evaluate(left) * Evaluate(right),
    _ => throw new()
};

// New operations are functions, not classes
string ToInfix(Expression expr) => expr switch
{
    Literal(var v) => v.ToString(),
    Addition(var l, var r)
        => $"({ToInfix(l)} + {ToInfix(r)})",
    Multiplication(var l, var r)
        => $"({ToInfix(l)} * {ToInfix(r)})",
    _ => throw new()
};
```

---
layout: two-cols-header
---

## The Memento Pattern: State Snapshots Made Trivial

Records with `with`-expressions made immutable snapshots a language feature.

::left::

<div class="font-mono text-xs tracking-widest uppercase text-slate-400 mb-3">Before — 40 lines of boilerplate</div>

```csharp
// The originator
class Editor
{
    private string _text = "";
    private int _cursor = 0;

    public string Text => _text;
    public int Cursor => _cursor;

    public void Type(string text)
    {
        _text = _text.Insert(_cursor, text);
        _cursor += text.Length;
    }

    public void MoveCursor(int position)
        => _cursor = Math.Clamp(position, 0, _text.Length);

    // Memento inner class
    public class Memento
    {
        public string Text { get; }
        public int Cursor { get; }
        internal Memento(string text, int cursor)
            => (Text, Cursor) = (text, cursor);
    }

    public Memento Save() => new Memento(_text, _cursor);

    public void Restore(Memento memento)
    {
        _text = memento.Text;
        _cursor = memento.Cursor;
    }
}

// Usage
var editor = new Editor();
editor.Type("Hello");
var snapshot = editor.Save();
editor.Type(" World");
editor.Restore(snapshot); // Back to "Hello"
```

::right::

<div class="font-mono text-xs tracking-widest uppercase text-amber-600 mb-3">After — 10 lines, built-in</div>

```csharp
// The type IS the memento
record EditorState(string Text, int Cursor);

class Editor
{
    private EditorState _state = new("", 0);

    public string Text => _state.Text;
    public int Cursor => _state.Cursor;

    public void Type(string text)
        => _state = _state with
        {
            Text = _state.Text.Insert(_state.Cursor, text),
            Cursor = _state.Cursor + text.Length
        };

    public void MoveCursor(int position)
        => _state = _state with
        {
            Cursor = Math.Clamp(position, 0, _state.Text.Length)
        };

    // Save / restore are one-liners
    public EditorState Save() => _state;
    public void Restore(EditorState saved) => _state = saved;
}

// Usage is identical, but implementation is 75% smaller
// The record gives us: equality, hashing, printing, immutability
```

<div class="mt-4 text-sm text-slate-500">
  Records provide structural equality, immutability, and <code>with</code>-expressions for free.
</div>

---
layout: two-cols-header
---

## Null Object Pattern: Language-Level Null Safety

Nullable reference types eliminated the need for explicit "null object" implementations.

::left::

<div class="font-mono text-xs tracking-widest uppercase text-slate-400 mb-3">Before — defensive hierarchy</div>

```csharp
interface ILogger
{
    void Log(string message);
    void LogError(string message);
}

class ConsoleLogger : ILogger
{
    public void Log(string message)
        => Console.WriteLine($"[INFO] {message}");

    public void LogError(string message)
        => Console.WriteLine($"[ERROR] {message}");
}

// The "null object" — does nothing
class NullLogger : ILogger
{
    public void Log(string message) { }
    public void LogError(string message) { }
}

// Usage with explicit null handling
class Service
{
    private readonly ILogger _logger;

    public Service(ILogger? logger = null)
        => _logger = logger ?? new NullLogger();

    public void DoWork()
    {
        _logger.Log("Starting work");
        // ... work ...
        _logger.Log("Work complete");
    }
}

// Problem: you can still pass null!
// Problem: every interface needs its own NullX implementation
```

::right::

<div class="font-mono text-xs tracking-widest uppercase text-amber-600 mb-3">After — compiler-enforced safety</div>

```csharp
interface ILogger
{
    void Log(string message);
    void LogError(string message);
}

// No NullLogger needed — use the null-coalescing operator
class Service
{
    private readonly ILogger _logger;

    public Service(ILogger logger)
        => _logger = logger; // or: ?? throw new()

    public void DoWork()
    {
        // Null-safe invocation
        _logger.Log("Starting work");
    }
}

// Caller with optional logging
var service = new Service(consoleLogger);

// Or skip logging entirely
var service2 = new Service(new NullLogger());
// But now the compiler warns if you pass null!

// Even better — extension method for conditional logging
service.DoWork();
logger?.Log("Done"); // Compiler knows this is safe
```

<div class="mt-4 text-sm text-slate-500">
  <code>#nullable enable</code> makes null a compiler error, not a runtime surprise.
</div>

---

## SOLID Revisited

| Principle | Status                          | Root cause                                             |
| --------- | ------------------------------- | ------------------------------------------------------ |
| **SRP**   | Still valid, narrowed           | The _module_ changed; feature slices are now the unit  |
| **OCP**   | Mostly obsolete                 | Solved recompilation cost — just modify and redeploy   |
| **LSP**   | Timeless, less triggered        | Sound mathematics; inheritance is used far less        |
| **ISP**   | Half valid                      | Fat C++ headers gone; `IFoo` per class is ceremony     |
| **DIP**   | Concept valid, practice harmful | Spawned `IFoo`-for-everything and DI container overuse |

<v-click>

<div class="mt-5 bg-amber-50 border-l-4 border-amber-500 rounded-r px-4 py-3 text-sm text-amber-900">
  The principles most tightly coupled to <strong>physical compilation cost</strong> aged worst.
  SRP and LSP survive because they're not about compilation — they're about reasoning.
</div>

</v-click>

---
layout: section
background: '#1A1F2E'
class: 'text-white'
---

<div class="font-mono text-sm tracking-widest uppercase text-amber-500 mb-3">Part II</div>

# What Remains Essential

_Patterns solving domain and structural problems_

---

## GoF Survivors — Why They Held Up

These survive because they solve **shape-of-the-problem** issues, not language gaps.

<v-clicks>

- **Facade** — Language features don't reduce the complexity behind the facade. Still the primary tool for wrapping third-party SDKs, legacy systems, and generated clients.

- **Proxy** — Lazy loading, access control, and cross-cutting concerns not expressible as middleware still need object-level interception. Source generators make compile-time proxies practical.

- **Composite** — Tree-structured domains (permission hierarchies, expression trees, UI component trees) require uniform treatment of leaves and branches. No language feature provides this.

- **Flyweight** — Shared immutable high-volume state (compiled `Regex`, `FrozenDictionary`, canonical strings) still needs explicit pooling.

</v-clicks>

---

## Domain Patterns — The Most Durable Category

These predate OOP and will outlast it. Records make the syntax easier; the _concepts_ are unchanged.

<v-clicks>

- **Aggregate** — The consistency boundary is a domain decision, not a framework concern. Everything inside is transactional; everything outside is eventually consistent.

- **Value Object** — `record Money(decimal Amount, string Currency)` — defined by attributes, no identity. Records make it trivial to write; the modelling insight remains vital.

- **Domain Event** — `OrderPlaced`, `PaymentFailed` — decouples what happened from everything that reacts to it. Foundational for Marten projections and event sourcing.

- **Domain Service** — Operations spanning multiple aggregates that don't belong on any single root, and cannot leak into application or infrastructure layers.

</v-clicks>

---
layout: two-cols
---

## Resilience & Distributed Patterns

These didn't exist in the GoF era. They emerged with cloud-hosted, network-dependent systems.

### Resilience (Polly / .NET 8)

<v-clicks>

- **Circuit Breaker** — fail fast, protect thread pools
- **Retry + Backoff** — transient faults with jitter
- **Bulkhead** — isolate resource pools
- **Timeout** — bound every async operation

</v-clicks>

::right::

<br><br>

### Distributed Systems

<v-clicks>

- **Outbox** — solve the dual-write problem
- **Saga** — coordinate across service boundaries
- **CQRS** — separate read / write pressure
- **Producer / Consumer** — `Channel<T>` backpressure

</v-clicks>

<v-click>

<div class="mt-5 bg-red-50 border-l-4 border-red-400 rounded-r px-3 py-3 text-sm text-red-800">
  These are <strong>operational necessities</strong>, not design preferences.<br>
  Skipping them has failure-mode consequences, not just code quality ones.
</div>

</v-click>

---
layout: section
background: '#1A1F2E'
class: 'text-white'
---

<div class="font-mono text-sm tracking-widest uppercase text-amber-500 mb-3">Part III</div>

# What Wolverine Absorbs

_Patterns as manual plumbing vs. framework design_

---

## Wolverine's Absorption Surface

| Manual pattern                             | Wolverine native                                          |
| ------------------------------------------ | --------------------------------------------------------- |
| MediatR / custom in-process mediator       | `IMessageBus.InvokeAsync` / `PublishAsync`                |
| Handler registration ceremony              | Convention discovery — `Handle(Cmd cmd)` is enough        |
| Custom handler middleware / behaviours     | `IMessageMiddleware` + source generation                  |
| Manual Outbox implementation               | `AutoApplyTransactions` — the default                     |
| Manual transaction coordination            | `[Transactional]` / auto-detected via Marten session      |
| Manual dead letter handling                | Built-in DLQ, replay via management API                   |
| Manual saga / process manager              | Wolverine `Saga` — `StartOrContinueWith`, `CompleteUsing` |
| `IHostedService` for background processing | Wolverine _is_ the hosted service                         |
| `BackgroundWorker` / timer scheduler       | Cascading `SchedulePublishAsync` messages                 |

---
layout: two-cols-header
---

## Concurrency: From Lock to Queue

The old instinct: protect shared state with a runtime primitive.
The Wolverine instinct: eliminate the race architecturally.

::left::

<div class="font-mono text-xs tracking-widest uppercase text-slate-400 mb-3">Before — runtime guard</div>

```csharp
private int _counter;

void Handle(IncrementCmd cmd)
{
    // Fights the race at execution time.
    Interlocked.Increment(ref _counter);

    // Still: ordering, visibility,
    // and distributed state problems remain.
}
```

::right::

<div class="font-mono text-xs tracking-widest uppercase text-amber-600 mb-3">After — architectural constraint</div>

```csharp
// Configure once at startup
opts.LocalQueueFor<IncrementCmd>()
    .MaximumParallelMessages(1);

// Handler now runs one-at-a-time by design.
// No primitive. No race to protect against.
// Degrades linearly, not as a cliff.
```

<v-click>

<div class="mt-4 bg-amber-50 border-l-4 border-amber-500 rounded-r px-3 py-2 text-sm italic text-amber-900">
  The race is eliminated before the handler runs,<br>not guarded inside it.
</div>

</v-click>

---
layout: two-cols-header
---

## Scheduling: BackgroundWorker → Cascading Messages

`BackgroundWorker` was designed in 2005 for WinForms UI thread marshalling — not server-side scheduling.

::left::

<div class="font-mono text-xs tracking-widest uppercase text-slate-400 mb-3">Before — hidden background thread</div>

```csharp
var worker = new BackgroundWorker();
worker.DoWork += (s, e) => {
    while (true) {
        DoScheduledWork();
        Thread.Sleep(
            TimeSpan.FromMinutes(5));
    }
};
worker.RunWorkerAsync();
// ✗ Invisible to diagnostics
// ✗ Lost on process restart
// ✗ No retry on failure
```

::right::

<div class="font-mono text-xs tracking-widest uppercase text-amber-600 mb-3">After — self-scheduling message</div>

```csharp
class RunScheduledWork
{
  public static (DoWork, RunScheduledWork)
    Handle(RunScheduledWork _)
  {
    var next = new RunScheduledWork();
    return (
      new DoWork(),
      next.ScheduledAt(
        DateTimeOffset.UtcNow
          .AddMinutes(5)));
  }
}
// ✓ Durable  ✓ Observable  ✓ Retryable
```

---
layout: section
background: '#1A1F2E'
class: 'text-white'
---

<div class="font-mono text-sm tracking-widest uppercase text-amber-500 mb-3">Part IV</div>

# The Repository Question

_`IDocumentSession` as the new seam_

---

## What Repository Actually Does

| Purpose                             | Status with Wolverine + Marten                                           |
| ----------------------------------- | ------------------------------------------------------------------------ |
| Hide the persistence mechanism      | **Absorbed** — `IDocumentSession` _is_ the seam, not a leaky abstraction |
| Unit of work / transaction boundary | **Absorbed** — `AutoApplyTransactions` owns the handler boundary         |
| Named, reusable query operations    | **Partially valid** — still useful for queries shared across slices      |
| Enforce aggregate root access rule  | **Weakened** — now a convention, not a structural constraint             |

<v-click>

<div class="mt-5 bg-amber-50 border-l-4 border-amber-500 rounded-r px-4 py-3 text-sm text-amber-900">
  In a Wolverine / Marten / VSA stack, the Repository sits closer to the
  <strong>Absorbed</strong> column than the <strong>Architectural</strong> one.
  The concept is valid; the `IOrderRepository` wrapper is not pulling its weight.
</div>

</v-click>

---
layout: two-cols-header
---

## Practical Recommendation

::left::

### Drop these

<v-clicks>

- `IOrderRepository` with `Save` / `Load` / `FindBy`
- Mock-friendly interfaces wrapping `IDocumentSession`
- Repository as a test seam — use Testcontainers instead
- Manual `SaveChanges` / transaction orchestration

</v-clicks>

::right::

### Keep these

<v-clicks>

- Inject `IQuerySession` / `IDocumentSession` directly
- Named query methods for logic shared across slices
- Marten compiled queries (`ICompiledQuery<T>`) on hot paths
- Aggregate root discipline as a code review convention

</v-clicks>

<v-click>

```csharp
// Idiomatic Wolverine / Marten handler
public static async Task<OrderDto> Handle(
    GetOrder query,
    IQuerySession session,
    CancellationToken ct)
    => await session.LoadAsync<Order>(query.OrderId, ct);
```

</v-click>

---

## The Meta-Pattern

Every expired practice was solving for **expensive feedback loops**.

| Feedback loop     | Old cost                      | Now                             |
| ----------------- | ----------------------------- | ------------------------------- |
| Compilation       | Minutes to hours              | Seconds                         |
| Test run          | Real DB, fixtures, licenses   | Testcontainers, in-process      |
| Deployment        | Build, package, ship binaries | Push, CI, done                  |
| Production change | Risky, hard to reverse        | Feature flags, canary, rollback |

<v-click>

<div class="bg-amber-50 border-l-4 border-amber-500 rounded-r-lg px-5 py-4 my-4 text-lg italic text-amber-900">
  When the feedback loop gets fast enough, the practice that managed the cost
  of the slow loop becomes the bottleneck.
</div>

</v-click>

---

## Key Takeaways

<v-clicks>

1. **Ask the forcing function question** — what specific constraint did this solve?

2. **The GoF book was written for C++ without lambdas** — many patterns are language compensation, not timeless wisdom.

3. **SOLID's most cited principles are tied to compilation economics** — SRP still holds, OCP mostly doesn't.

4. **Domain patterns are the most durable** — Aggregate, Value Object, Domain Event predate OOP and will outlast it.

5. **Wolverine absorbs infrastructure patterns, not domain ones** — stop writing `BackgroundWorker` schedulers and manual outboxes.

6. **In a Wolverine / Marten stack, `IDocumentSession` is the repository** — a wrapper adds a layer with no new capability.

7. **Current practices have expiry dates too** — microservices-by-default and orchestration frameworks are solving today's constraints.

</v-clicks>

---
layout: cover
background: '#0F1117'
class: 'text-left'
---

<div class="font-mono text-xs tracking-widest uppercase text-amber-500 mb-5">
  // C# 12 · .NET 8 · Wolverine 2.x · Marten 7.x
</div>

# Questions?

<p class="text-slate-400 mt-4 text-lg leading-relaxed">
  A filterable reference with all 53 patterns —<br>
  organised by category and status — is available as a companion document.
</p>

<p class="text-slate-600 mt-10 text-base italic">
  Retire the patterns that compensate for constraints that no longer exist.<br>
  Keep the ones that reason about your domain.
</p>

<style>
  h1 { color: #E8E9F0 !important; border: none !important; letter-spacing: -0.03em; }
</style>
