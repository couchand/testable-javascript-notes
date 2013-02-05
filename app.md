Building a testable application
===============================

Components of a test framework:

 * production code
 * test code
 * test runner

Gotcha!  Empty string is falsey!!!

The ideal JS test framework would do all equality checks
strictly with `===` by default.  There's no good use case
for the lazy equal.

If you're thinking about running your tests through `sed`
to generate them, then you're not writing the right tests.

If you're too dogmatic about TDD you can get into a
situation where you have to decide between bad and worse.
That's where BDD intends to differ.

A good example of this is a `numberToWord` function.  Do
you write tests for every possible number?

Tests should be as declarative as possible.  It should be
possible to immediately verify that a test is correct.

BDD Terminology
---------------

 * `suite` => `describe`
 * `test` => `it`
 * `assert.equal(a,e)` => `expect(a).to.eql(e)`

Hooks
-----

How do you test a method that calls another method?

Stub!  But don't stub directly.  Add a option hook that
defaults to the method being called.

But!
----

Don't make arbitrary decisions just to make things testable.

Singletons
----------

The DOM is one big singleton.  It's a PITA to test.

Secret members
--------------

Encapsulate private data and functions, BUT! its untestable.






