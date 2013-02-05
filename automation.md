Automation Tools
================

Headless
--------

Running headless browsers to use those browser-based test
frameworks.

 * PhantomJS
   * uses WebKit
 * Zombie.js
   * uses Node and custom browser
   * super fast
   * but very different from real browsers.

Load live pages on the web.  Or, point it at your test
runner, so now your tests are run headless, returning
results to the command line.

Grunt
-----

JavaScript build tool.  Plugins for all the major test
frameworks.  Gruntfile.js == Makefile

Mocha plugin uses notify for OS-level popup!

Now all our test frameworks make status codes for CI!

Plugin to wrap JSHint as part of built process!  Setup
different settings for the test files and the production
code.

Just like any well-behaved build tool, it respects the exit
code of each step.  Failures stop the build process in its
tracks.  Makes CI easy!

Run your code quality checks on commit!

Selenium
--------

Web browser automation.  Controls an actual instance of a
real, standard-issue web browser.  Not unique (like Zombie)
or slightly off (like Phantom).  Script fake user
interaction.  Inspect (scrape) the page for results.

Allows for writing high-level integration and acceptance
tests that actually exercise the full application, but can
be easily automated.

Continuous Integration
======================

Compile and run all tests automatically on each commit.

 * Travis <http://travis-ci.org>
 * Jenkins <http://jenkins-ci.org>

Poor-man's version.  Set a Git pre-commit hook.

	> vi .git/hooks/pre-commit

	#!/bin/bash
	grunt
