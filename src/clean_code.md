# Clean Code

from book: *Clean Code A Handbook of Agile Software Craftsmanship* by Robert C. Martin.

## What?

*"I like my code to be elegant and efficient. The
logic should be straightforward to make it hard
for bugs to hide, the dependencies minimal to
ease maintenance, error handling complete
according to an articulated strategy, and performance close to optimal so as not to tempt
people to make the code messy with unprincipled optimizations. Clean code does one thing
well."*

~ Bjarne Stroustrup

Clean code: elegant and efficient.


*"Clean code is simple and direct. Clean code
reads like well-written prose. Clean code never
obscures the designer’s intent but rather is full
of crisp abstractions and straightforward lines
of control."*

~ Grady Booch, author of Object
Oriented Analysis and Design with
Applications

Clean code: simple & direct -> readibility.

*"Clean code can be read, and enhanced by a
developer other than its original author. It has
unit and acceptance tests. It has meaningful
names. It provides one way rather than many
ways for doing one thing. It has minimal dependencies, which are explicitly defined, and provides a clear and minimal API. Code should be
literate since depending on the language, not all
necessary information can be expressed clearly
in code alone."*

~ “Big” Dave Thomas, founder
of OTI, godfather of the
Eclipse strategy

Clean code: has tests(unit and integration), minimal dependencies, minimal API.


Ron Jeffries, author of Extreme Programming
Installed and Extreme Programming
Adventures in C#:

In recent years I begin, and nearly end, with Beck’s
rules of simple code. In priority order, simple code:

• Runs all the tests;

• Contains no duplication;

• Expresses all the design ideas that are in the
system;

• Minimizes the number of entities such as classes,
methods, functions, and the like.


## Meaningful Names

- Names should reveal intentions(without comments).
- Avoid Disinformations.
- Make meaningful distinctions.
- Use pronounceable names.
- Use searchable names, avoid single letter, unique enough while searching.
- Avoid encoding, e.g. Hungarian Notation.
- No need prefix, each naming should be small enough. E.g. if it's already represented by the package names, no need to put it in classes name.
- Avoid mental mapping, avoid readers thinking about something else about the names.
- Class/Object/Struct or any kind of object/type should be named with **noun**.
- Methods/Functions should be named with **verb**.

## Functions

- Small
- Do one thing
- Top-Down approach, read code from top to down
- Function arguments max to 3, more than 3 need some kind of refactor if possible.
- Avoid output argument, argument passed-in, and also acts as output value. Use return value instead.
- Avoid flag argument, e.g. passing boolean as argument, implying the function do more than 1 thing.
- More than 3 arguments should be made into its own class/object.
- Avoid side effects.
- Output arguments: avoid it, in early days before OOP it might be needed, but in OOP output argument is only for `this` object, not the arguments passed-in.
- Command Query Separation: Commands: Mutations, Query: Read-only.
- Prefer exceptions to returning codes.

## Comments

- Don't comment bad code, rewrite it.
- Make code self explainatory
- Good comments: 
  - Legal comments(e.g. license).
  - Informative and details of the code, if cannot be explain by function name.
  - Making clarification.
  - Warnings
  - TODO
  - Amplify importance of something for certain consequences.
  - Documentation for public APIs

## Objects and Data Structure

- Make attributes private, access them with methods.
- Objects: data and behaviour coupled, Data structure: data and behaviour separated.
  - *"Procedural code (code using data structures) makes it easy to add new functions without changing the existing data structures. OO code, on the other hand, makes it easy to add new classes without changing existing functions."*
  - *"Procedural code makes it hard to add new data structures because all the functions must change. OO code makes it hard to add new functions because all the classes must change"*
  - Know when to use procedural code, when to use OOP. Not everything is an object.
- Law of Demeter: *The method should not invoke methods on objects that are returned by any of the allowed functions. In other words, talk to friends, not to strangers.*
- Train Wrecks: Long chain of calls should be avoided, prefer them into each lines.
- Data Transfer Object: Class/Data Structure with public members and no functions/methods.
  - Much like *bean* object in Java. DTO is one kind of a bean.
  - Beans are ([source](https://semanticreatures.com/2018/10/16/java-beans-and-dtos)):
    - all properties are private (and they are accessed through getters and setters).
    - they have zero-arg constructors (aka default constructors).
    - they implement the Serializable Interface.

## Errors Handling

- Use exceptions rather than return codes.
- Use unchecked exceptions
  - The price of checked exceptions is violation of open-close principle of clean arch.
  - Using checked exceptions make you need to expose the type of exceptions to higher level callers.
  - Changes to exception types mean changes to higher level caller.
- Use checked exceptions if you make it mandatory for the caller to handle the errors, such that the error is supposed to be strongly typed and specific to the dependencies(e.g. libraries, some libraries define their own error types).
- Don't return null, returning null causing NPE. Throw exceptions instead of null, or wrap with something else.
- Don't pass null, you never know what the function/method does when you pass null, but for sure it's gonna be NPE. As method/function provider, consider checking null and handle them with exceptions.

## Boundaries

- Careful when introducing 3rd party library, cause we never know what APIs they provide and what are the impact to the maintainer of the code depending on it. Some client/caller might be arbitrarily invoking anything from said 3rd libraries without care, causing something unexpected. E.g. Map, which expose anything that you can do with map. If you need this Map anywhere in the code, provide abstractions/wrappers around the library and only expose things you need. Also it's not recommended to cast things from literal map, use generics instead.
- Learning tests, spending time learning new 3rd party lib by testing it, or write tests on it.
- Minimize interfaces to 3rd party codes only to the ones we really need, exposing naked dependencies might introduce side effects(but not always, depends on how the lib's APIs written).

## Unit Tests

Write tests to automate testing so the tests can be easily reproduced.

From Unit tests raised TDD.

TDD: Write unit tests first, before production code.

3 Laws of TDD:
- *You may not write production code until you have written a failing unit test.*
- *You may not write more of a unit test than is sufficient to fail, and not compiling is failing.*
- *You may not write more production code than is sufficient to pass the currently failing test*

*Test code is just as important as production code*.

Unit Tests make the code maintainable because it guards from any changes. Any changes to the existing code must break something, and that's the tests.

*One Assert per Test*

*Single concept per Test*

### FIRST Principles for Clean Tests
- Fast: Tests should run fast, because it needs to be run frequently. Slow tests prevent from frequent run.
- Independent: Tests should not depends on each other, should be stateless(not shared with other tests), should run in any order, and not causing cascading errors.
- Repeatable: Tests can be run in any environment, without anything needed(except the code & tests itself). No network, no nothing needed.
- Self-Validating: Tests should be either *Passed* or *Failed*, no need to trace the logs nor any information, simply from whether it passed or failed.
- Timely: Unit tests should be written *just before* the productions code(in timely fashion). The timing is adjusted, as long as tests first. If you write production code first, you can end up not writing tests to it.

*Opinion: This is why unit tests involve mock to make the tests fast and independent, since mock cut the IO/syscalls dependencies, and can be repeat multiple times anywhere without setting things up.*

*Opinion: With clear separation between business rules/logics/domains, mocks aren't really needed. Mocks tend to exist when infrastructures part of the code mixed with non-infrastructure codes.*

## Classes

- Classes should be small.
- Refer to SRP in SOLID Principles.
- *We want our systems to be composed of many small classes, not a few large ones. Each small class encapsulates a single responsibility, has a single reason to change, and collaborates with a few others to achieve the desired system behaviors.*
- Cohesion: instance variables are used in every methods(maximizing the cohesion, coupling between attributes and methods).
- Cohesion gives Smaller Classes, when class loose cohesion, separate it into different classes.
- Isolating from changes(OCP, ISP, DIP): concrete classes should not be depended upon directly by client/caller, instead caller/client should depends on abstractions/interfaces.

## System

- Separate apps/systems dependencies from using it, by having initialization process for dependencies or other components needed by the runtime. Runtime only depends on those objects created at initialization phase.
- One of ideal way to separate initialization is to put it in `main`.
- Use Abstract Factory pattern for initializing/creating these system dependencies.
- Use dependency injection for system dependencies, an object should not responsible for its own dependencies creation/initialization, instead it's up to higher level part of code handling it(e.g. main function/method).
- Another place to put dependencies is container, through IoC(Inversion of Control) principle.
- Pass dependencies into object's constructor, instead of creating/initializing them inside said object. Or use specialised object to contain all dependencies and pass it along, called container(ApplicationContext).

*An optimal system architecture consists of modularized domains of concern, each of which is implemented with Plain Old Java (or other) Objects. The different domains are integrated together with minimally invasive Aspects or Aspect-like tools. This architecture can be test-driven, just like the code.*

Communicate using DSL(Domain Specific Language) to narrowing the gap between domain experts and implementor.


## Kent Beck's simple design rules

It's related to how TDD is implemented.

1. Run All The Tests, related to TDD, all tests should passed to be able to verify the program. Low coupling and High cohesion. Tests are all specified first. before next steps.
2. Refactoring, after running all the tests, implements the code and keep refactoring, while checking the tests. If we broke anything, keep refactoring until tests all assesrted, keep repeating the steps by refactoring the codes and cleaning up the tests to adjust the new changes.

## Concurrency
Myths and misconceptions:
- Myth: *Concurrency always improve performance*
  - Actual: it improves performance only when there're wait points on blocking components(e.g. IOs, or some syscalls).
- Myth: *Design doesn't change when writing concurrent programs*. 
  - It could be
  - (e.g *What's the color of your functions?*)
- Myth: *Understanding concurrency issues is not important when working with a container such as a Web or EJB container.*
  - How to deal with requests coming from the web servers?
  - How to synchronize the shared data?, 
  - How to ensure thread safety?

What concurrencies will have:
- Incurs some overhead if not used correctly.
- Correct concurrency is complex.
- Bugs are not repeatable/reproducible easily.
- Often require fundamental changes.
- Incurs thread-safety problems(e.g. data races, some race conditions)

Concurrency Defense:
- SRP
- Limit the scope of data: carefully sharing data, especially mutables ones, even don't share mutable data at all on multiple threads.
- Copy the data, so it's not shared.
- Threads should be independent.