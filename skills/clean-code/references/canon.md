# Cross-Canon Principles

This reference distills durable software-engineering principles from peer-reviewed papers, books by recognized practitioners, and large-scale industry experience reports. It is an original synthesis with attribution; no source text is reproduced.

Use this when `chapter-map.md` (which is scoped to one book) is not enough — for cross-cutting decisions about complexity, change, scale, optimization, and design intent.

## How To Use This Reference

- Read end-to-end once to recognize the principles.
- During work, jump to the section that matches the decision in front of you.
- Treat each principle as a question to apply, not a rule to recite.

## Principles By Theme

### Modules Should Hide Decisions That Change

Source: Parnas, *On the Criteria To Be Used in Decomposing Systems into Modules*, CACM 1972.

The criterion for splitting a system is not the steps in a flowchart. It is the design decisions most likely to change. Each module hides one such decision; its interface reveals as little as possible about the choice it owns.

Agent application:

- Before adding a module, name the decision it is hiding. If you cannot, the module probably does not earn its keep.
- When a single change forces edits in many places, the boundary is wrong; the changing decision is not encapsulated.
- Stable interfaces matter more than clever internals. Internals can change later if the interface kept the secret.

### Programs Should Be Composed Correctly, Not Debugged Into Correctness

Source: Dijkstra, *The Humble Programmer*, ACM Turing Award lecture, 1972.

Reliability is achieved by avoiding bugs, not finding them. The discipline is intellectual humility: choose tractable designs over heroic ones, prove the small claim, and do not rely on debugging to recover from sloppy thinking.

Agent application:

- Prefer designs you can reason about over designs you would have to instrument to trust.
- When a function is hard to test, treat that as a design signal, not a testing problem.
- A change that "should work" but cannot be reasoned about clearly is not finished.

### Optimize The Critical 3 Percent, Not The Other 97

Source: Knuth, *Structured Programming with go to Statements*, ACM Computing Surveys, 1974.

The famous "premature optimization is the root of all evil" line is half a sentence. The full claim is that small efficiencies should be ignored most of the time, but the critical small fraction of code where performance matters deserves careful, measured optimization.

Agent application:

- Default to the clearest implementation.
- When performance is asserted as a requirement, measure first, identify the hot path, and optimize that path with tests covering the simpler behavior.
- Do not let "we might need it fast later" justify obscure code today.

### Essential Complexity Stays; Accidental Complexity Is Yours To Remove

Source: Brooks, *No Silver Bullet — Essence and Accident in Software Engineering*, IEEE Computer, 1987.

Software difficulty splits in two. Essential complexity comes from the problem itself: the domain, the constraints, the changing world it models. Accidental complexity comes from how we chose to build it: tooling, ceremony, indirection, framework gravity. New tools mostly attack the second kind, which is a smaller and shrinking share of the total.

Agent application:

- Distinguish "this is hard because the problem is hard" from "this is hard because we made it hard." Only the second is yours to fix in passing.
- Be skeptical of order-of-magnitude productivity claims for any single tool, language, or framework.
- When a refactor reduces accidental complexity but cannot reduce the essential complexity, say so honestly.

### Simple Is Not The Same As Easy

Source: Hickey, *Simple Made Easy*, Strange Loop 2011.

Simple means one fold, one concern, not braided with others. Easy means familiar, near at hand. The two often diverge: many easy tools are not simple, and many simple choices are not easy at first.

The verb is "complect": to braid or interleave concerns that do not need to be combined. State and identity, configuration and policy, transport and domain, retry and business logic — each pair is commonly complected.

Agent application:

- During review, name what is being complected, not just what is "coupled."
- Prefer the simple choice even when the easy choice is faster today, when the change will live a long time.
- A library that is easy to start with but hard to disentangle later is a future cost, not a current win.

### Deep Modules Beat Shallow Wrappers

Source: Ousterhout, *A Philosophy of Software Design*, 2nd ed. 2021.

A module's value is the complexity it hides minus the complexity its interface adds. Deep modules — small interface, substantial implementation — give callers leverage. Shallow modules — thin pass-throughs, configuration shells, single-method classes that just rename — usually add cost without hiding anything.

Two related ideas:

- **Two sources of complexity.** Dependencies (you cannot understand or change a piece in isolation) and obscurity (vital information is not apparent from the code or its interface). Every design decision should reduce one without inflaming the other.
- **Strategic vs tactical programming.** Tactical work optimizes for shipping today and accumulates design debt. Strategic work treats every change as an investment in the system's clarity and changeability. Sustained tactical-only coding compounds into rewrite pressure.

Agent application:

- Before introducing a new layer, check whether it actually hides complexity or just relabels it.
- When the request is tactical, do the tactical thing — but if you are repeatedly forced to ship tactical patches in the same area, name it as a strategic problem rather than continuing to patch.
- Comments are valuable when they document interfaces, invariants, and intent that the code cannot express. A "no comments" rule is not a clean-code rule.

### Knowledge Has One Home

Source: Hunt and Thomas, *The Pragmatic Programmer*, 2nd ed. 2019.

DRY is not "do not type the same characters twice." It is "every piece of knowledge has one authoritative representation." Duplicated *knowledge* — the same business rule, the same constant, the same validation — is the dangerous form because the copies drift.

The companion idea is orthogonality: independent things should be expressible independently. Code is "shy" when it does one thing and ignores the rest of the system. Orthogonal systems make change local instead of viral.

Agent application:

- Before deduplicating, check that the duplication encodes the same rule. Coincidental similarity is not duplicated knowledge and should not be merged.
- When a small change ripples across many unrelated modules, the system has lost orthogonality somewhere; name the missing boundary.

### Depend On Abstractions, Compose Behavior

Sources: Gamma, Helm, Johnson, Vlissides, *Design Patterns*, 1994. Martin, *Design Principles and Design Patterns*, 2000 (SOLID, named by Feathers ~2004).

Two principles outlast any individual pattern:

- Program to an interface, not an implementation.
- Favor object composition over class inheritance — inheritance binds subclass internals to parent internals and breaks encapsulation.

SOLID restates this for the design of small modules:

- **S**ingle responsibility — one reason to change per module.
- **O**pen/closed — extend behavior without modifying tested code.
- **L**iskov substitution — subtypes must be safely substitutable for their base type.
- **I**nterface segregation — many narrow interfaces beat one fat one.
- **D**ependency inversion — high-level policy must not depend on low-level detail; both depend on abstractions.

Agent application:

- Treat deep inheritance hierarchies and forced base-class extension points as design smells worth questioning.
- When extracting an abstraction, check that it is named for what callers need, not for what the implementation does.

### Smells Are Prompts To Look, Not Permission To Rewrite

Source: Fowler, *Refactoring: Improving the Design of Existing Code*, 2nd ed. 2018.

A code smell is a surface signal that something underneath may be wrong. The discipline is: notice the smell, decide whether the current task can safely address it, and use a behavior-preserving refactoring backed by tests if so. Drive-by rewrites are not refactoring.

Agent application:

- For each smell you notice, ask: is it in the scope of this task, did my change introduce it, and can I fix it safely with the current tests?
- Mention out-of-scope smells in your report instead of fixing them silently.

### Adapt The Rule To The Stack; Do Not Impose One Style

Source: McConnell, *Code Complete*, 2nd ed. 2004.

There is no universal best process or tool. Construction (design + coding + unit testing + integration) is the irreducible activity. Practices should be experimentally adapted to the language, ecosystem, team, and risk profile.

Agent application:

- Ecosystem idioms beat generic advice. Go is not Java; SQL is not Python; shell is not TypeScript.
- When you propose a new practice, name what you expect to improve and how you would tell whether it worked.

### Code Lives Longer Than You Expect; Design For That Timescale

Source: Winters, Manshreck, Wright, *Software Engineering at Google*, 2020.

Programming is writing code that works now. Software engineering is sustaining code over time and across many contributors. Three forces change everything once you cross from one to the other:

- **Time.** Dependencies, behaviors, and interfaces will be relied on by someone you have never met.
- **Scale.** Practices that work for one engineer break at thousands.
- **Tradeoffs.** There is no abstract "best." There are only choices justified by measured constraints.

A consequence the book makes explicit, often called Hyrum's Law:

> With a sufficient number of users of an interface, every observable behavior of your system will be relied on by somebody.

Agent application:

- For any externally observable behavior — including incidental ones like field ordering, timing, error messages, or sort stability — assume someone depends on it. Change with care or version the contract.
- When the codebase is small today but the interface is public, design as if it were already large; future you cannot recover the contract you leaked.
- "It works on my branch" is a programming claim. "It will keep working as the codebase changes around it" is the engineering claim.

## Cross-Reference Pressure Test

Use these scenarios to verify the principles are being applied together rather than in isolation.

| Scenario | Pull from |
| --- | --- |
| Splitting a monolith service | Parnas (information hiding), Ousterhout (deep modules), GoF/Martin (depend on abstractions), SWE@Google (Hyrum's Law on internal APIs) |
| Wrapping a vendor SDK | Parnas, GoF (program to interface), Hunt/Thomas (orthogonality), Fowler (boundary smells) |
| Performance-sensitive change | Knuth (measure, then critical 3%), Brooks (essential vs accidental), Ousterhout (strategic clarity) |
| Cleaning up a legacy area | Fowler (smells + behavior-preserving refactoring), Dijkstra (compose correctly), McConnell (construction discipline) |
| New shared internal API | SWE@Google (Hyrum's Law, time, scale), Parnas (hide what changes), Hickey (do not complect transport with domain) |
| Tempted by a new framework | Brooks (no silver bullet), Hickey (simple vs easy), McConnell (experiment, measure, do not absolutize) |

## Sources

Original works only. Use these for the authoritative reading; the synthesis above is a working agent's distillation, not a substitute.

- Parnas, D. L. *On the Criteria To Be Used in Decomposing Systems into Modules.* Communications of the ACM, 15(12), 1972.
- Dijkstra, E. W. *The Humble Programmer.* ACM Turing Award lecture, 1972.
- Knuth, D. E. *Structured Programming with go to Statements.* ACM Computing Surveys, 6(4), 1974.
- Brooks, F. P. *No Silver Bullet — Essence and Accident in Software Engineering.* IEEE Computer, 20(4), 1987.
- Gamma, E.; Helm, R.; Johnson, R.; Vlissides, J. *Design Patterns: Elements of Reusable Object-Oriented Software.* Addison-Wesley, 1994.
- McConnell, S. *Code Complete*, 2nd ed. Microsoft Press, 2004.
- Hunt, A.; Thomas, D. *The Pragmatic Programmer*, 20th Anniversary Edition. Addison-Wesley, 2019.
- Fowler, M. *Refactoring: Improving the Design of Existing Code*, 2nd ed. Addison-Wesley, 2018.
- Martin, R. C. *Design Principles and Design Patterns.* Object Mentor, 2000. (SOLID acronym attributed to Michael Feathers.)
- Hickey, R. *Simple Made Easy.* Strange Loop conference talk, 2011.
- Ousterhout, J. *A Philosophy of Software Design*, 2nd ed. Yaknyam Press, 2021.
- Winters, T.; Manshreck, T.; Wright, H. (eds.) *Software Engineering at Google: Lessons Learned from Programming Over Time.* O'Reilly, 2020.
