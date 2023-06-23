# My Software Engineering Principles

_NOTE: Work in progress._

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

- Do small but constant improvements, like updating documentation or addressing
  a compilation warning.
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
- Tools must be convenient and easy to use.
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
- Document contracts/interfaces. Use things like [Doxygen][doxygen] and
  [PlantUML][plantuml]. Make it look nice and easy to understand. Include
  examples. There must be no need to look into an implementation to understand
  how to use APIs.
- If the observed behaviour (the implementation) does not match the promised one
  (the contract), then the implementation is wrong and must be fixed.
- As a client rely only on what is promised by the contract. Never rely on any
  particular unspecified behaviour. It will change and will break your code.
- Changes to public APIs affect [Versioning](#versioning).
- Make APIs hard to use wrong. Be explicit. Express value semantics with value
  types, e.g. [`std::chrono::seconds`][cpp-chrono] instead of `int`. Express
  ownership with [RAII][cpp-raii] and [smart pointers][cpp-ptr].
- Test only public APIs and observable behaviour. Test that the implementation
  fulfils the contract, not implementation's internals.

[doxygen]: https://www.doxygen.nl
[plantuml]: https://plantuml.com
[cpp-chrono]: https://en.cppreference.com/w/cpp/chrono/duration
[cpp-raii]: https://en.cppreference.com/w/cpp/language/raii
[cpp-ptr]: https://en.cppreference.com/w/cpp/memory
