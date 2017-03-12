A simple library providing argument validation methods and Code Contracts helpers.

NuGet package [![NuGet](https://img.shields.io/nuget/v/Narvalo.Cerbere.svg)](https://www.nuget.org/packages/Narvalo.Cerbere/)
available [here](https://www.nuget.org/packages/Narvalo.Cerbere/).

Usage Guidelines
================

### Preconditions

A precondition is a condition that must be true at the beginning of the execution
of a method; argument validation is one such condition, and certainly the most
common.

We provide four static classes to write preconditions:
- `Require` uses Code Contracts preconditions and throws an exception on failure.
- `Enforce` only throws an exception on failure. It complements `Require` when
  the condition is too complex for the Code Contracts tools.
- `Expect` only uses Code Contracts preconditions.
- `Demand` uses Code Contracts preconditions and debug assertions (`Debug.Assert`).

Only `Require` and `Enforce` method calls survive in retail builds, that is
when selecting the Release configuration and when not defining
the `CONTRACTS_FULL` symbol.

If you verify your code with the Code Contracts tool,
- for public methods, use `Require` or `Enforce` when
  a condition is compulsory, use `Expect` otherwise
- for internal/private methods, use `Demand` when a condition is compulsory
- be very careful with protected methods, if your class is not sealed
  you can't know in advance if the caller will satisfy the condition.

In any case, **never** use `Demand` to guard a public method.

### Check points

None of these assertions will survive in retail builds:
- `Check`: Code Contract + Debug.Assert

### Postconditions

### Class Invariants

### Unreachable code

### `ExcludeFromCodeCoverage`

### `ValidatedNotNull`

Developer notes
===============

Requirements:
- Visual Studio 2015
- Code Contracts extensions

Before publishing the NuGet package, do not forget to update the version number
in nuspec and AssemblyInfo.

Notes
=====

### `Narvalo.Check.Unreachable()`

Adapted from a [MSDN blog](https://blogs.msdn.microsoft.com/francesco/2014/09/12/how-to-use-cccheck-to-prove-no-case-is-forgotten/).

Unfortunately, CCCheck will still complain with _CodeContracts: requires unreachable_.
You can safely suppress this warning and, later on, if you reach the "unreachable"
point, CCCheck will produce a different warning: _This requires, always
leading to an error, may be reachable. Are you missing an enum case?

### `Narvalo.ValidatedNotNullAttribute`

Adapted from a [blog post](http://geekswithblogs.net/terje/archive/2010/10/14/making-static-code-analysis-and-code-contracts-work-together-or.aspx).
