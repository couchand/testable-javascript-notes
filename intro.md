Writing Testable Javascript
===========================

Mike - Mr. Rogers - Squeaky Shoe

Why are we here?
---------------

 * front-end apps are getting BIG
 * but the testing hasn't caught up

Who is Mike?
------------

 * degrees from institutions
   * but not much test experience
 * consultant at Bocoup
   * wide variety of philosophies
   * introducing testing to devs

Outline
-------

 * terms
 * code org
 * asynch
 * units
 * refactoring

Aside
-----

This is still an unsettled topic.  Terms and best-practices may be ill-defined.

Terminology
-----------

Types of tests:

 * unit
   * context based
   * function, line, method
   * not concerned with interaction of two things
 * functional
   * does not require initializing the entire application
 * integration
   * scripting user interaction with complete app
   * screen-scraping to be user's eyes

Classifications

 * sca - detect
   * syntax errors
   * formatting/whitespace
   * bad patterns/practices
     * unused code
     * assignment instead of conditional
   * code style
 * fuzz
   * netflix - chaos monkey
 * smoke
 * regression

Unit Test
---------

The steps:

 * setup
   * precondition validation?
 * execution
 * validation
 * teardown

Assertions.

Defines an expectation.  Does this statement evaluate to true?

But we can add more advanced semantics.  Does this array have
the expected elements?  Does this function throw an error?
Does the string match this regular expression?

Test Driven Development
-----------------------

Structure.

 * write test
 * see failure
   * important to prove the value of the test
 * write bare minimum to pass
   * to avoid introducing untested features
 * refactor

Purpose.

 * think about the api first
   * other devs (or yourself) are the users
 * organize code to be testable
   * much easier than shimming in later
   * testable == modular == maintainable
 * avoid technical debt
   * never leave testing for another day
 * maintain codebase assurances (regression!)
 * not always appropriate
   * speculative/exploratory development is hindered
 * technically-enforced docs
   * tests are better docs than comments
   * executable tests are better than unexecutable examples
   * wrong comments are worse than no comments
 * emotionally satisfying!!!
   * red to green

Behavior Driven Development
---------------------------

Dan North.

Purpose.

 * avoid "brittle" tests
 * wholistic thinking about product/service
 * encourage non-devs in req spec
   * conceptually: think in terms of interaction instead of assertion
   * literally: use tools that give non-devs speaking power
     * Cucumber!!!

Distinction from unit testing.

 * TDD and BDD map to different DSLs.

For instance:

	// TDD
	suite("String", function() {
	  setUp(function() {
	  });
	  module(#trim()", function() {
	    test("removes whitespace", function() {
	      equal("  foobar  ".trim(), "foobar");
	    });
	  });
	});

	// BDD
	describe("String", function() { 
	  before(function() {
	  describe("#trim()", function() {
	    it("removes whitespace", function() {
	      "  foobar  ".trim().should.equal("foobar");
	    });
	  });
	});











Parking Lot
-----------

 * "use strict"
 * CoffeeScript
 * Test-Driven JavaScript Development - Christian Johansen
 * BusterJS
 * SinonJS
 * DuckDuckGo
 * GitHub linking projects (jQuery+Sizzle)
 * vim tabs!
 * .vimrc
 * UnderscoreJS
 * Ringmark - mobile testing
 * CommonJS


