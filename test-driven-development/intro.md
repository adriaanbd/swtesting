# Test Driven Development (TDD)

Writing tests is one of those things that separates the best from the rest. It allows for more complex and powerful software to be written with ease by achieving a productive *workflow*. A state of mind.

## Who created TDD?

TDD was developed or *rediscovered* in 2003 by American software engineer [Kent Beck](https://en.wikipedia.org/wiki/Kent_Beck) and exposed in the book *Test-Driven Development: By Example*. It's worth nothing that Beck is also the creator of [extreme programming](https://en.wikipedia.org/wiki/Extreme_programming), and one of the 17 original signatories of the Agile Manifesto (the founding document for [agile software development](https://en.wikipedia.org/wiki/Agile_software_development)).[[1]](https://en.wikipedia.org/wiki/Test-driven_development)

## What is TDD?

First and foremost, TDD is based on the concept of an automated test: a program that tests another program, generally testing a specific portion of that program, whether it is a function, a method, a class or a mix of these. The portion of the program that is being tested is called *system under test*. 

There are many different kinds of automated tests, e.g. unit tests, integrations tests, end-to-end tests, among others, but they all share the same foundation: writing code that works.

TDD is a software development process that relies on small iterative development cycles, wherein requirements are turned into specific test cases and code is written to pass the tests. This is a different process from implementing a feature list and then writing a test for it.

## What is the primary goal of TDD?

One view states that the goal is specification and not validation; meaning that it's one way to think through your requirements or design before you write your cuntional code. Another view states that TDD is a coding technique to write clean code that works.[[2]](http://www.agiledata.org/essays/tdd.html)

## Test First Development(TFD) and TDD

TFD and TDD go hand in hand in development, being that is based in a series of tests. The first step is to add a minimum test, with just enough code for it to fail. Then you run the tests, often the complete test suite or a subset of it, to ensure that the new tests do in fact fail. The code is then updated to pass the new tests and the tests are run again. If the tests fails, the code needs to be updated and the code needs to be run again. Once the tests pass, the next step is to start over.

The steps of TFD, can me summarized as:
1. Add a test
2. Run the tests
3. Make a little change
4. Run the tests
5. If tests fail, go to step 3.
6. If tests pass, development stops and cycle starts again.

As shown below:

![[Steps of TFD]](http://www.agiledata.org/images/tddSteps.jpg)


### In summary, TDD = TFD + Refactoring.

Instead of writing functional code first and then testing the code as an afterthought, a test code is written before the functional code.  This is done in very small steps – one test and a small bit of functional code at a time.  

A programmer taking a TDD approach would refuse to write a new function until there is first a test that fails because that function isn’t present. In fact, they refuse to add even a single line of code until a test exists for it.  Once the test is in place they then do the work required to ensure that the test suite now passes.

## Conclusion

TDD means we create some sort of test first, write a sample to make the test run, run it, and watch it fail. This step is import, as one doesn't really know if the test is correct until one verifies that it fails. Thus, verify that the test fails first. Then, write code to make the test pass.