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
