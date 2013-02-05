Pitfalls of Testing
===================

Coverage
--------

 * too little
   * lots of setup, not many tests
   * missing edge cases
   * be thorough!
 * reliance on superficial analysis
   * line coverage !== logical coverage
   * helpful, but semantics are important
   * often a single line has several possible modes
   * overconfidence in your tests...
   * peer-review tests
 * too much
   * testing third-party libraries
   * testing internals (like underscore methods)
   * increased maintenance costs
   * slows development cycle
   * write tests mindfully
   * functional coverage !== logical coverage

Maintenance
-----------

 * stubbing
   * using lots of stubs and mocks
   * couples test to internal structure
   * couples the test to the relationship between objects
   * might obscure brittle design
   * changing source requires housekeeping of relevant stubs
   * minimize coupling between modules to minimize stubs
 * integration
   * meaningless assertions (why is that important?)
   * e.g., some element has a class name that code relies on
   * tests are brittle
   * tests lose value as documentation
   * be critical of your assertions

Testing Stochastic Processes
----------------------------

 * Good luck!
 * Remove the source of randomness
 * Deterministically test the parts you can
 * Fuzz test the integration


