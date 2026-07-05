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

## The Pattern Decision Matrix

<div class="text-sm text-slate-500 mb-5">A reusable mental model for evaluating any pattern — not just the ones on these slides.</div>

<div class="grid grid-cols-2 gap-3" style="height:65%">
  <div class="rounded-xl p-4 border-2 border-red-200 bg-red-50 flex flex-col">
    <div class="font-mono text-xs font-bold text-red-600 uppercase tracking-widest">Retire</div>
    <div class="text-xs text-red-400 mb-2">Language limitation · forcing function gone</div>
    <div class="text-sm text-red-900 flex-1">The language now does this natively. Delete the pattern.</div>
    <div class="mt-3 font-mono text-xs text-red-500">Strategy · Builder · Visitor · Null Object</div>
  </div>
  <div class="rounded-xl p-4 border-2 border-blue-200 bg-blue-50 flex flex-col">
    <div class="font-mono text-xs font-bold text-blue-600 uppercase tracking-widest">Replace</div>
    <div class="text-xs text-blue-400 mb-2">Language limitation · better idiom exists</div>
    <div class="text-sm text-blue-900 flex-1">Intent is valid; expression changes. Write the idiom, not the ceremony.</div>
    <div class="mt-3 font-mono text-xs text-blue-500">IStrategy → Func&lt;T&gt; · IVisitor → switch · IBuilder → record with</div>
  </div>
  <div class="rounded-xl p-4 border-2 border-amber-200 bg-amber-50 flex flex-col">
    <div class="font-mono text-xs font-bold text-amber-600 uppercase tracking-widest">Absorb</div>
    <div class="text-xs text-amber-400 mb-2">Infrastructure concern · framework handles it</div>
    <div class="text-sm text-amber-900 flex-1">The framework or runtime provides this. Rolling your own duplicates it.</div>
    <div class="mt-3 font-mono text-xs text-amber-500">Singleton→DI · Outbox→Wolverine · Chain→Middleware</div>
  </div>
  <div class="rounded-xl p-4 border-2 border-purple-200 bg-purple-50 flex flex-col">
    <div class="font-mono text-xs font-bold text-purple-600 uppercase tracking-widest">Retain</div>
    <div class="text-xs text-purple-400 mb-2">Domain / structural · problem persists</div>
    <div class="text-sm text-purple-900 flex-1">No language or framework change removes domain complexity. Keep it.</div>
    <div class="mt-3 font-mono text-xs text-purple-500">Aggregate · Value Object · Circuit Breaker · CQRS</div>
  </div>
</div>

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

## SOLID Revisited

| Principle | Status | Root cause |
|---|---|---|
| **SRP** | **Evolved** — cohesion unit shifted | From class-level to vertical slice / bounded context; the insight stands |
| **OCP** | Obsolete for most; survives in niches | CD removes the cost OCP managed. Still real for SDKs, plugins, and regulated release cycles |
| **LSP** | Timeless, less triggered | Sound mathematics; inheritance is used far less now |
| **ISP** | Half valid | Fat C++ headers gone; `IFoo`-per-class is ceremony, not design |
| **DIP** | Direction valid; ceremony harmful | Depend on abstractions at module boundaries — only where real substitution exists |

<v-clicks>

<div class="mt-4 bg-amber-50 border-l-4 border-amber-500 rounded-r px-4 py-3 text-sm text-amber-900">
  The principles most coupled to <strong>physical compilation cost</strong> aged worst.
  SRP and LSP survive because they reason about <em>behaviour</em>, not build economics.
</div>

<div class="mt-2 bg-slate-100 rounded-lg px-4 py-3 text-sm text-slate-600">
  <strong>On DIP specifically:</strong> dependency <em>direction</em> at module boundaries is vital.
  Extracting <code>IFoo</code> for every class is premature abstraction masquerading as inversion.
  Ask: is there a concrete substitution you'll actually make?
</div>

</v-clicks>

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

<div class="mt-3 bg-slate-800 text-slate-200 rounded-lg px-3 py-2 text-xs font-mono leading-relaxed">
  <span class="text-green-400">// Wolverine message log</span><br>
  [14:30:00 INF] Scheduled RunScheduledWork @ 14:35:00<br>
  [14:35:00 INF] Executing RunScheduledWork<br>
  [14:35:00 INF] Succeeded. Next scheduled: 14:40:00<br>
  <span class="text-yellow-400">[14:40:00 WRN] Attempt 1 failed — retrying in 30s</span>
</div>

---
layout: section
background: '#1A1F2E'
class: 'text-white'
---

<div class="font-mono text-sm tracking-widest uppercase text-amber-500 mb-3">Part IV</div>

# The Repository in Modern .NET

_When `DbContext` and `IDocumentSession` change the calculus_

---

## What Repository Actually Does

| Purpose | EF Core | Marten |
|---|---|---|
| **Hide persistence** | `DbContext` is already the seam; a wrapper adds a layer | `IDocumentSession` *is* the seam — document-oriented by design |
| **Unit of work** | `DbContext` tracks changes natively | `AutoApplyTransactions` owns the boundary at handler level |
| **IQueryable leakage** | Real concern — LINQ expressions bleed into domain code | Not applicable — returns documents, not `IQueryable<T>` |
| **Named queries** | Specification / query methods still earn their place | Compiled queries (`ICompiledQuery<T>`) or query methods |
| **Aggregate root rule** | Structural constraint has value here | Convention only — no type system enforcement |

<v-click>

<div class="mt-4 bg-amber-50 border-l-4 border-amber-500 rounded-r px-4 py-3 text-sm text-amber-900">
  In both ecosystems the session/context <em>is</em> the repository. The question is whether a wrapper adds enough to justify the layer.
  In EF Core the <code>IQueryable</code> abstraction gap is the most defensible reason to wrap.
  In Marten, that gap does not exist.
</div>

</v-click>

---
layout: two-cols-header
---

## Practical Recommendation

::left::

### EF Core

<v-clicks>

- Wrapping `DbContext` is justified **only** if `IQueryable<T>` leaks into domain code
- Everything else — UoW, transactions, lifetime — `DbContext` already provides
- If you wrap, keep it thin: named query methods, no `IQueryable` exposure
- Aggregate root discipline still deserves a structural constraint here

</v-clicks>

::right::

### Marten + Wolverine

<v-clicks>

- Inject `IDocumentSession` / `IQuerySession` directly in handlers
- No `IQueryable` leakage — the seam is already clean
- Use compiled queries (`ICompiledQuery<T>`) for hot-path shared logic
- Aggregate root discipline becomes a code review convention, not a wrapper

</v-clicks>

<v-click>

```csharp
// EF Core — wrap only if IQueryable would leak
public Task<Order?> FindAsync(Guid id)
    => ctx.Orders.FirstOrDefaultAsync(o => o.Id == id);

// Marten — inject the session directly
public static Task<Order?> Handle(GetOrder q, IQuerySession s, CancellationToken ct)
    => s.LoadAsync<Order>(q.OrderId, ct);
```

</v-click>

---

## Retiring Abstractions Means Retiring Mock-Heavy Tests

Removing wrapper interfaces changes how you must test.

<div class="grid grid-cols-2 gap-4 mt-4">
<div class="bg-red-50 border border-red-200 rounded-xl p-4">

**Before — mock the repository**

```csharp
var repo = new Mock<IOrderRepository>();
repo.Setup(r => r.FindAsync(id))
    .ReturnsAsync(order);
// Tests the mock, not the behaviour
```

</div>
<div class="bg-green-50 border border-green-200 rounded-xl p-4">

**After — test with real infrastructure**

```csharp
await using var host =
    await AlbaHost.For<Program>();
await host.Scenario(s =>
    s.Post.Json(new PlaceOrder(id)));
// Real DB · real handler · real outbox
```

</div>
</div>

<v-click>

<div class="bg-amber-50 border-l-4 border-amber-500 rounded-r px-4 py-3 text-sm text-amber-900 mt-4">
  <strong>Retiring the abstraction layer means retiring the mock-heavy unit test.</strong>
  Integration tests with Testcontainers or Alba become the primary quality gate.
  They are now fast enough to make this the right trade.
</div>

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

6. **The session/context is the repository** — `DbContext` in EF Core, `IDocumentSession` in Marten. Wrap only when `IQueryable` leakage is the concrete concern.

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
