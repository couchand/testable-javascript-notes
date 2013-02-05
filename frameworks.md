Testing Frameworks
==================

We'll look at four.  Three are browser-based, two are
node-based (Mocha is both).

 * QUnit
 * Jasmine
 * Mocha
 * NodeUnit

QUnit
-----

From the jQuery folks.  Minimal API.  Semantics are string
literals.  Flat test structure.  Async is semaphore-based.

 * `module`
   * `setup`
   * `teardown`
     * use `this`
 * `test`
   * name
   * expect
   * closure
     * assertions
 * `asyncTest`
   * name
   * expect
     * vital to asynchronous tests!
   * closure
   * `start`
     * the test is done...?
   * `stop`
     * increment semaphore to describe the number
       of `start`s to expect

Jasmine
-------

Specs.  More fully-featured matchers (assertions).
Functional test structure.  Nested describe blocks.
Async through polling.

 * `describe`
   * `beforeEach`
     * closure
   * `afterEach`
     * closure
 * nested `describe`s
 * `it`
   * name
   * closure
   * `expect`: matchers
 * async test
   * `runs`
     * closure
   * `waitsFor`
     * closure
     * poll name
     * poll failure timeout

NodeUnit
--------

Command-line.  Has a return code signifying status (great
for CI).  Text-only output (but pretty formatting).  Object
literal structure.  QUnit-like assertions.  Everything is
asynchronous.

 * `setUp`
   * closure accepts done callback param
 * `tearDown`
   * closure accepts done callback param
 * suite name
   * nested `setUp` and `tearDown`
 * test name
   * closure accepts test param
   * param has assertions and `done()`

Mocha
-----

Browser or command-line.  Flexible export naming.  Flexible
test structure.  Implicit asynchronous.  BYO assertions.

 * `suite` or `describe`
   * name
   * closure
   * `setup` or `before` and `beforeEach`
   * `teardown` or `after` and `afterEach`
 * `test` or `it`
   * name
   * closure
   * if test takes param, Mocha knows it's async
     * function has a length property: the num of params
     * does jQuery.each iterate over a function, then?

Assertion Libraries
===================

Define your expectations.  Really only for use with Mocha.

 * Should
   * <http://github.com/visionmedia/should.js> (Mocha peeps)
   * a TON of assertions
 * Expect.js
   * <https://github.com/LearnBoost/expect.js> (SocketIO peeps)
   * based on Should
   * minimal API
 * Chai
   * <http://chaijs.com>
   * flexible assertion styles
     * assert
     * expect
     * should (modifies `Object` prototype)

SinonJS
=======

Testing utility library.  All of these CAN be written, but
why bother re-writing them over and over again?

Spies
-----

Functions that record how they are invoked, great for
testing callback behavior.

	var successSpy = sinon.spy();

	puppy.fetch({
	  success: successSpy
	});

	ok( successSpy.callCount, '`success` method was called' );

Stubs
-----

Spies with more.  Simple behavior to test externally-triggered
codepaths.

	var badBark = sinon.stub(Puppy.prototype, 'bark');
	badBark.throws();

	var puppy = new Puppy();
	var squirrel = new Squirrel();

	puppy.see( squirrel );

	ok( badBark.callCount, '`bark` should have been called' );
	ok( !squirrel.scared, 'squirrel was not scared due to failed `bark`');

But!  Stubbing makes tests somewhat harder to maintain by
coupling the test code to the relationship between methods.

Mocks
-----

Stubs with more!  Baked-in expectations.

	var mock = sinon.mock(Puppy.prototype);
	mock.expects('bark').never();

	var puppy = new Puppy();

	puppy.see('rock');
	puppy.see('book');

	mock.verify();

	// teardown!
	// mock.release();

But!  It's tempting to write tests covering many units.

> With great power comes great responsibility.

Fakes
-----

Mocks for JavaScript built-ins.

 * Timers
   * `setTimeout` and `clearTimeout`
   * `setInterval` and `clearInterval`
   * `Date` constructor
 * AJAX
   * XHR
   * "Server"

Sandboxes
---------

An entire global state.

	// in setup
	this.sb = sinon.sandbox.create();

	// in teardown
	this.sb.restore();

Takes care of cleaning up all mocks, stubs, and spies.


