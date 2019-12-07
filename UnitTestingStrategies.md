# Unit testing strategies
### Planning for effective unit testing

## Agenda
* What are you unit tests and why do you do it?
* Why unit testing fails

## What is an automated unit test?

The term unit test has become overloaded and often are used to refer to multiple kinds of testing:

**Unit tests** are code developed for the purpose of testing portions of an application with aknown inputs and expeted outputs.

**Integration tests** testing multiple modules of an application together

**Functional tests** validate business requirements

**Technical unit tests** performance, memory, scalability

#### Why have unit tests?

* Promote sound programming practices
* Develop higher quality software
* More productive code reviews
* Code base that is easier to refactor
* Improves longevity of your code base
* Easier to on-board developers/teams
* Find bugs when it's least expensive to fix them
* Free your QA/QE staff from repetitive tasks to work on creative testing instead

## Why unit tests fail

A typical plan of "more is better", or writing tests for the sake of having tests, is a big cause of failure.

* Tests were commented out each time they broke
* Tests were bound to a database or other external sources that made them brittle
* The effort required to maintain the tests exceeds the perceived value
* The developer who worked on those tests is gone
* Tests weren't modified to match changes in the requirements
* We break things we don't test and test things we don't break
* The new leader doesn't want to waste money developing code that doesn't ship with the product

## Planning for success

### Set boundaries

* Decide what should and should not be included in your tests (e.g. external data sources)
* What layers of code will be tested?

### Decide the different types of testing you will have

* Manual
* Unit
* Performance
* UI Automation
* etc

### Design the tests

Every stakeholder should have a say in designing the tests, not just the developers (the inputs and expected outputs).

Relying on the developer to design them on their own often leads to failure or missed cases.

**Tests should document your business rules**

#### Who writes tests and when?

Needs to be established as a team. 

Tools can allow product owners/designers to write tests which the developers implement (SpecFlow).

Will the tests be written in sprint, as feature is developed, before a bug is fixed, before the product ships, or in response to support cases? Should be decided and known ahead of time.

### Decide when the tests will be executed

* Manually run by developers while making changes
* Executed automatically in CI/CD pipeline
* Gating criteria for integrated builds, shipping, and acceptance testing

### Transparency

Everyone should be able to see the results and everyone should be able to understand what is covered.

### Architecture

* Fixtures and mocks should be deliberate and reusable
* Architects should design complex fixtures and mocks
* Failing to manage the complexity of your test setup basically ensures that your unit tests will be abandoned
* Tests with lots of duplicated code are a shortcut to abandonment

> The setup should be reused when possible

Unit testing code will need to be refactored just like any other code.

Any coding standards applied to code should also be applied to unit tests.

### Oversight

* Unit tests should be a part of your code review process
* Automated test cases should be reviewed by whoever writes the business rules
* **Creating unit tests should be represented in stories or tasks and part of your 'definition of done'**
* Demonstrated with features in sprint demos, or handoff demonstrations

### Tooling

Organization should standardize the following

* Unit testing framework
* Mocking library
* Memory/preformance testing components
* CI/CD pipeline

### Code Coverage

|Without code coverage|With Code Coverage|
|---|---|
|May only be testing happy paths|Ensure that all code paths are covered|
|Don't know how complete your tests are|Recognize unreachable code|
||Track progress|

## How unit tests relate to the build

If unit tests don't compile, it should break the build. Test results should be part of the build output.

## Maintaining unit tests

Every developer should be able to add tests and fix broken tests. If your application is undergoing a technical uplift, shift in architecture, or refactor, your tests should do the same.

## End of life

Tests should be eliminated when 

* Supplanted by another test
* The team decides to replace this test with an automated UI test or some other form of test
* Functional design change makes the test obsolete

Decide who can remove tests and process needs to happen for that.

## Plan 

Must have a plan that incorporates all of this:

* Rules and best practices. Enforcement. Oversight
* Scope, tooling, structure
* Roles of developers, architects, testers, and designers
* Metrics
* Lifecycle of your tests

The entire team needs to buy into the plan, not just a few people. The plan should be documented. 

Skeptics, who don't see the value of unit tests, need to buy in as well. If they can't be brought on-board, you need to find ways to kick them off of the ship.
