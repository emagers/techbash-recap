# Getting to Green
### How to safely refactor legacy code

 > Refactoring is a disciplined technique for restructuring an existing body of code, altering its internal structure **without changing its external behavior**.

 > Legacy code is simply code without tests. Code without tests is bad code. 
 
 \- Michael Feathers

 ## Addressing Legacy Code

 The temptation is to get rid of it and start over. But even if legacy code is bad code:
 * It works
 * It is a known quantity
 * There may be a reason the solution isn't as simple as you think it could or should be

### If it works, why change it?

#### Bad code carries cost and risk

* What if it needs to be updated?
* What if it has a security issue?
* What if it is incompatible with an update that fixes a security problem?
* Reduce technical debt
* Be proactive, not reactive

#### [Martin Fowler has a refactoring book](http://refactoring.com/catalog)

## When Refactoring

> Whenever I do refactoring, the first step is always the same. I need to build a solid set of tests for that section of code. 

\- Martin Fowler

Unit testing is a way for developers to document the behavior of code.

* Authomated
* Independent
* No external dependencies
* Test individual methods or smaller
* Become our safety net for fearless refactoring

### Unit Testing Enables Change

The challenge of how to refactor legacy code becomes a challenge of how to unit test legacy code.

* Once it has tests, it isn't legacy code anymore
* We have a safety net
* We can fearlessly refactor
* We can just follow the catalog of business rules

#### It's okay to make private methods public for testing purposes

## Code Coverage

#### Measures code executed when unit tests run
* NOT amount of code tested

#### Good tool to find untested code (legacy code)
* Not covered == not tested
* Covered == possibly test (not gauranteed)

## Mutation Testing

.NET tool: `Stryker.NET`

Mutates code after unit tests are in place. If the tests still pass, you missed a unit test condition. Outputs a PIT report.

## Refactoring

1. Small, incremental changes
1. Safety net needed (tests)
1. Unit testing was necessary, but difficult

## Untestable Code

We want to add tests, but the code isn't making that easy
* Important methods are void, rely on side effects, etc.
* Log methods do too much to comprehend or require complicated and specific setup before the test can be executed.
* Other code or services are invoked from within, so the unit of code can't be easily isolated.

1. Start small
2. Test specific areas of methods and when confident, pull them out into their own methods
3. Rely on Mocks

 `Microsoft Fakes` can help when there aren't interfaces available to mock.

**This package should only be used only until the code can be refactored to use interfaces and proper mocking frameworks**

## Static Code Analysis

#### Inspects source code

* Code coverage
* Duplicated Code
* Complex Code
* Tightly coupled code
* Security issues
* Helps identify riskiest code
* Refactoring opportunities
* Also shows
   * Unused variables
   * Confusing code
   * Best practices
   * Coding standards

#### Enable and watch for warnings in Visual Studios build

## 5 Main Takeaways

1. Safety is the goal. Strive for fearless refactoring.
2. Unit tests are the best safety net for making code changes.
3. Use mutation testing to make sure your unit tests are actually covering what you need covered.
4. Use a combination of mocking tools and mocks to isolate your tests.
5. Make small, incremental, safe changes.

## Tools

* Unit testing framework
* Code coverage tool
* Mutation testing tool
* Mock frameworks
* Static analysis tool
* IDE refactoring tools
* Lots of patience

[Email](gene.gotimer@coveros.com)

[Website](https://hub.techwell.com/)