# My Software Engineering Principles

_Version 1.2.0._

This is my personal view on how to do software development.

I have created this page to express what I expect from software projects, how
I approach software engineering and let others know what to expect from me.

## Limitations

Everything has its limits. So do these principles.

- They are biased. I developed software mostly in C++ and mostly for the
  automotive domain. And the principles reflect this.
- They are not always applicable. They are not laws of nature, after all.
  There always needs to be a pragmatic approach and a clear distinction between
  what can be done and what actually should be done.
- They are not set in stone. As my experience evolves, the principles can
  also change.

## Entropy

The code quality tends to deteriorate. Counteract!

- Do small but constant improvements, like updating documentation, addressing
  a compilation warning, or making code slightly more efficient.
- Keep the code clean. Do not let the [Broken Windows Theory][bwt] to kick in.

[bwt]: https://en.wikipedia.org/wiki/Broken_windows_theory

## Versioning

Use [Semantic Versioning](https://semver.org/). Especially for libraries.

- Do backwards compatible API changes. Any code correctly working with `A.B.C`
  must keep working with `A.B'.C'` for each `B'` greater than `B`.
- Introduce new APIs alongside the old ones. Give clients time to switch.
  Mark the old ones [`[[deprecated]]`][cpp-depr].
- Postpone breaking API changes for as long as possible. Ideally, such breaking
  change is just removing the deprecated functionality.

[cpp-depr]: https://en.cppreference.com/w/cpp/language/attributes/deprecated

## Standard Language

Write standard compliant code.

- Disable compiler extensions. We want to have portable code.
- Enable as much warnings as possible, and treat them as errors.
  Let a compiler help you.
- Compile code with as many compilers as you can. Different compilers generate
  different diagnostics and let you find more issues in the code.
- Compile code with different optimisation settings. Make sure the code works
  the same way in release, debug etc. builds.

## Intended Usage

Use tools and technologies as intended.

- One way to define "as intended" is something like "how the software industry
  usually does it". For example, [`.clang-format`][clang-fmt] configuration file
  is usually put in a repository root.
- Do not invent non-standard ways to do common tasks.
- Constrain the [NIH syndrome][nih].
- Tools must be convenient and easy to use. If not, fix them or find new ones.
- Know your tools.

[clang-fmt]: https://clang.llvm.org/docs/ClangFormat.html
[nih]: https://en.wikipedia.org/wiki/Not_invented_here

## Software Quality Gating

Everybody makes mistakes. Catch them early.

- Design with testability in mind. Make mockable interfaces, employ dependency
  injection etc.
- Set up a gating system. No commit must squeeze in without being reviewed and
  passing automatic software quality checks.
- Ensure the [Standard Language](#standard-language).
- Have automated tests. At least unit tests. Ideally, also system tests and more.
- Use static code analysis tools, e.g. [`clang-tidy`][clang-tidy].
- Use [code sanitisers][sanit] to catch UB, memory leaks etc. Run tests with
  sanitisers enabled!
- Enforce code formatting, e.g. with [`clang-format`][clang-fmt].

[clang-tidy]: https://clang.llvm.org/extra/clang-tidy
[sanit]: https://github.com/google/sanitizers

## Interface Design

Program towards an interface, not an implementation.

- Produce modular designs with clear module boundaries and interfaces. There
  must be no other way to interact with a module but via its interface.
- Define a module contract/interface before starting its implementation. A
  contract is a set of rules and agreements which define what does the module
  do, what does it expect from its users and what does it promise to deliver. An
  interface (API) is a part of a contract and defines technical details on how
  to interact with a module.
- If the observed behaviour (the implementation) does not match the promised one
  (the contract), then the implementation is wrong and must be fixed.
- As a client, rely only on what is promised by the contract. Never rely on any
  particular unspecified behaviour. It will change and will break your code.
- Changes to public APIs affect [Versioning](#versioning).
- Make APIs hard to use wrong. Be explicit. Express value semantics with value
  types, e.g. [`std::chrono::seconds`][cpp-chrono] instead of `int`. Express
  ownership with [RAII][cpp-raii] and [smart pointers][cpp-ptr].
- Follow the principle of least surprises.
- Test only public APIs and observable behaviour. Test that the implementation
  fulfils the contract, not implementation's internals.

[cpp-chrono]: https://en.cppreference.com/w/cpp/chrono/duration
[cpp-raii]: https://en.cppreference.com/w/cpp/language/raii
[cpp-ptr]: https://en.cppreference.com/w/cpp/memory

## Developer Documentation

Help fellow developers. Tell how to use your stuff.

- Always document public APIs.
- Make it look nice and easy to understand. Include examples.
- There must be no need to look into an implementation to understand how to use
  APIs.
- Use things like [Doxygen][doxygen] and [PlantUML][plantuml].
- Bad documentation repeats what the code says. Good documentation complements
  the code. It describes "why?" not "what?" and provides the context to the code.

[doxygen]: https://www.doxygen.nl
[plantuml]: https://plantuml.com

## Software Design and Architecture

Prioritise maintainability, testability, traceability and extensibility. The
code is read, analysed and modified much more often than written from scratch.

- Mistakes in software architecture are the hardest and most costly ones to fix.
- There is no one-size-fits-all architecture. Each approach has its trade-offs.
- Make evolutionary architectures with low coupling and high cohesion.
- Postpone important (potentially irreversible) decisions to the latest point
  possible, when there is more information to make a better choice. Or make such
  things reversible by e.g. abstracting them away behind an interface.
- Set up a theory of operation. Describe how a software system should work, what
  are the invariants, what is the data flow etc.
- Do a prototype to show a feasibility of an architecture. Then throw it away.
  Never promote a prototype to the final solution.
- Separate concerns. Never mix business logic, UI, data access layer etc.
- Fail fast. Do not let the system run in an inconsistent state.
- Single-threaded event loop based applications usually work well and are easy
  to reason about.

## Development Process

Be agile.

- Kanban is usually better than Scrum. Waterfall is rarely the answer.
- Planning and designing everything upfront is dumb. Not planning and designing
  anything upfront is even dumber.
- Do not overspecify user stories. Often there is no need for more than a header
  and a couple of sentences to point into the desired direction.
- Engage developers into product ownership and decision making. They should have
  enough context to be able to derive what needs to be done, suggest ideas,
  discuss alternatives, and come up with a solution.
- Developers must test the code they are writing.

## Common Solution to Common Problems

It is in everyone's best interest to solve a problem well once and then let
everyone adopt the solution.

- Establish common abstractions, conventions and vocabulary.
- Provide reusable documentation and training materials.
- Identify and extract reusable code to libraries.
- Let different development teams concentrate on adding business value, not
  reimplementing the common things over and over again.
- Provide domain types -- small, robust and tested vocabulary types to base the
  the business logic upon.
- Standardise and reuse infrastructure code like IPC, monitoring or application
  lifecycle management.
- Support architecture decisions with various primitives (building blocks). It
  should be easy for developers to stay in line with the defined architecture.


## Performance

Do not prematurely (micro-) optimise for performance.

- Functionality and code readability are usually more important.
- Use proper data structures and algorithms. This is what affects the system
  performance the most.
- Do not do obviously inefficient things, e.g. do not do unnecessary copies.
- Optimise based on profiling results.
