Code Organization
=================

Organizing code is about keeping parts separate.

Closures
--------

Simplist: function.

Function scope in JS means we can enclose local variables
within a closure to keep them local.
We can enclose functions within a closure to make them
private methods.

You can use all sorts of weird characters to make an
anonymous function an expression:

	(function() {
	  console.log('foobar');
	})();

	(function() {
	  console.log('foobar');
	}());

	+function() {
	  console.log('foobar');
	}();

	~function() {
	  console.log('foobar');
	}();

	!function() {
	  console.log('foobar');
	}();

Some people lead their javascript with a semi-colon to
avoid issues when concatenating.  But that's probably
solving the wrong problem!

Why a closure?
--------------

Encapsulate variables and functions as private.

Shadow variables from a containing scope.

IIFE - immediately invoked function expression

Namespacing
-----------

Make a global object, expose only the functions that must
be public as properties on that single global object.

Your app should only set a single global variable.  Play nice.

 * Separate setup code from execution code.
 * The code that declares the application can run immediately.
 * The code the executes the application must wait for ready().

Moving forward
--------------

More files!  Break code up into the smallest modular pieces.

A ten-thousand line file (e.g. jQuery) is unusable.

Revealing Module
----------------

Return a value from your IIFE!

	var foo = (function() {
	  return {
	    bar: function() {}
	  }
	})();

	foo.bar();

Module initialization.

	var myApp = window.myApp = window.myApp || {};

The variable myApp is always the same as the window version,
and it will be initialized if not already.

That way, if many modules are being built into the same
namespace, the files don't have to be loaded or
concatenated in any particular order.

But!
----

Now all these private members are shielded from the tests
just like they are from the public.

The privacy cuts both ways.

We'll get to fixing that, but not yet.

Constructors
------------

What is a constructor?  It doesn't return anything
explicitly.  Also we name it with a leading capital.
You want to call it with `new`.  You can access `this`.

	var Person = function( firstName, lastName ) {
	  this.firstName = firstName;
	  this.lastName = lastName;
	};

	Person.prototype.introduce = function() {
	  console.log( 'My name is ' + this.firstName + ' ' + this.lastName );
	};

	var r = new Person('Rebecca', 'Murphey');

	r.introduce();

You can functionally get the same thing with:

	var Person2 = function( firstName, lastName ) {
	  this.firstName = firstName;
	  this.lastName = lastName;
	  this.introduce = function() {
	    console.log( 'My name is ' + this.firstName + ' ' + this.lastName );
	};

But!  There is a difference.

	(new Person()).introduce === (new Person()).introduce
	// true

	(new Person2()).introduce === (new Person2()).introduce
	// false

All instances of the prototype version have a reference to
the exact same method.  But each version of the second has
its own copy.  Much less performant, introduces test issues.

More ways to invoke a function
------------------------------

Directly invoking.  The same as calling a method on the
`window` object (or `global` for Node).

Calling a method on an object.  The special variable `this`
is set to the receiver of the method call, no matter where
the function was initially actually defined.

Let's use `call` and `apply`.

First `call`.  A method defined on functions.  First param
is the value of `this`.  The rest are passed to the function.

Also `apply`.  First param is also target `this`, and the
second method is an array with the list of params.

	var introduce = function(greeting) {
	  console.log( greeting + ' ' + this.name + '.' );
	}

	var ben = {
	  name: 'Ben'
	}

	ben.introduce = introduce;
	ben.introduce('Hello,');
	// Hello, Ben.

	introduce('Hello,');
	// Hello, undefined.

	introduce.call( ben, 'Hello,' );
	introduce.apply( ben, ['Hello,'] );
	// Hello, Ben.

Finally, the constructor invocation.  Use the keyword `new`
before the function name.  The value of `this` is set to a
new object.

Using the `new` keyword, you don't even need the parens
for the function invocation!

	var ben = new Person;
	// valid!

Checking the constructor
------------------------

Use `instanceof` or `#constructor`.

	ben instanceof Person;
	ben.constructor === Person;

Prototype chain
---------------

When you look for a mthod or property on an object, if it
is not an _own property_, the system must look up to the
object constructor's prototype.  If not defined there,
continues looking up the prototype chain until we reach
the base Object.

Use `hasOwnProperty` to determine where on the prototype
chain a particular method or property lies.

	ben.hasOwnProperty('foobar');
	// false

	ben.foobar = 'baz';
	ben.hasOwnProperty('foobar');
	// true

Encapsulation
-------------

Use the prototype to describe the behavior of a Widget.

This organized code to be *testable*.  Doing too much on
`document#ready` make code untestable.

Separation of concerns
----------------------

MVC.  Separate the data binding from the UI.

One class to represent widget, one class to represent
the data binding/fetching.  Now you can swap out the data
source without changing the widget.

File organization
-----------------

Keeping files separate is great, but you don't want to have
manually keep track of dependencies and have a huge list of
script tags on your page.

What's wrong with this?

 * Implicit dependencies
 * Relies on global leakage
 * Module authors have to manually handle collisions (e.g. `jQuery.noConflict`)
 * Tension between development and deployment

jQuery concatenates files.  Is there a better (more dynamic
way to do it?)

AMD &amp; RequireJS.

 * modularity
 * encapsulation
 * dependency management

Aside: where to put script tags?
--------------------------------

In the head.

 * Loads sequentially
 * Better for dependencies
 * Worse for performance

At the bottom or the body.

 * Loads in parallel
 * Better for performance
 * Dependencies, what?

AMD - asynchronous module definition.
-------------------------------------

An API pattern for loading project files.

 * Require.js
 * Curl.js (Cujo?)

What is good about these libraries?

 * Explicit dependencies per module
 * No need for global declarations
 * Consumer controls names of dependencies
 * Support best practices for both development and deployment

Example:

	// moduleA.js
	define([], function() {
	  return { modulesStuff: 1 };
	});

	// moduleB.js
	define([], function() {
	  return { modulesStuff: 2 };
	});

	// main.js
	require(['moduleA', 'moduleB'], function(modA, modB) {
	  // modA and modB hols the modules.
	  // nothing is added to the global namespace!
	});

There's also an apparently synchronous version.

	define(function(require){
		var modA = require('moduleA');
		var modB = require('moduleB');
	});

RequireJS decompiles the function with `toString` and
finds these require calls to load them asynchronously.

How does RequireJS work?
------------------------

Dynamic script injection.

 * Browser requests index.html
 * index.html has a single require.js `script` tag
 * Browser requests require.js
 * require.js looks for a `data-main` attribute on script tag
 * require.js dynamically injects `script` tag for main.js
 * Browser requests main.js
 * main.js specifies top-level dependencies, e.g. app.js
 * require.js dynamically injects `script` tag for app.js
 * Browser requests app.js
 * app.js specifies further dependencies
 * require.js dynamically injects `script` tag for module.js
 * Browser requests module.js
 * *all necessary dependencies have been resolved*

Common dependencies are only loaded once, no matter how
many times they are required.

Wait a second?!?!?!
-------------------

That's still making all those separate requests!  The page
still ends up with all those `script` tag in the head!

Let's use an optimizer!

r.js
----

Compiles the JavaScript and all dependencies for deployment.

	node r.js -o js/config name=app out=js/production.js

You end up with a single minified file with all needed code!







