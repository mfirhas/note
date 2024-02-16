# Test-Driven Development

source: My opinions about TDD

TDD is one of the hottest topic in programming community. It frequently discussed and debated in each generations. Some pros and some cons to it. It originates from Kent Beck's practice on Extreme Programming practice and popularizing TDD practice with that.

What TDD really is about?

Here are some definitions of TDD I gathered from many sources(internet, forum, group chat, book, youtube, etc):
- Development practice emphasizes on writing test code first, before production code.
- A practice to achieve modular and good design in writing software.
- A disciplined and flows in development to guide devs into making code more correct and maintainable.
- An effort into making a testable and maintainable software.
- Iterations in development with Red, Green, and Refactor phases.
- Part of Clean Code practices.

Here are some opinions from devs who cons on it:
- It's a cult practice, writing tests before any code exist, can't make sense of this.
- Writing tests took too long to implement slowing deliverability.
- Diminishing return.
- *"You're paid only for working/deployed code"*.
- Achieving 100% test coverage is wasting time.
- I just want to implement the solution right-away.

I think each of them have their own experience with TDD.

I myself agree with all of above definitions because the benefit is real. But, I have a little different approach in doing TDD compared to most practitioners. If TDD is really about writing tests first, literally, then Im not really doing that(in my TDD). 

To me, TDD doesn't strictly to write tests first, instead what come first are APIs/public interfaces. Then after the APIs/public interfaces firmly defined, we write tests based on them. Surely the tests will fail because you're only testing APIs/public interface with no implementation yet. We write the implementation right after tests written. After each implementation written, test again, keep going until all are satisfying both APIs/public interfaces definitions and tests passed.

My <s>TDD</s>BDD approach:
1. Define APIs/public interfaces.
2. Write tests on the APIs/public interfaces.
3. Write the implementations.

It's more closely related to BDD, because what driving the behaviours are APIs and public interfaces. Hence I prefer to call this practice as BDD.

I agree with cons no #1 above, but im not calling it cult, it's just weird to write tests before anything really exist. What should I write?, Do I just create dummy object with methods before Im really sure that's the object/methods I need?, how can I write tests before Im sure how my program will behave?, If I just write tests first guessing the object and their methods, well, what if after a bunch of tests written, turn out the APIs changed, all of those tests written need to be adjusted altogether. If I already defined and firmed on how the APIs and public interfaces look like, then writing tests will be more simple and adjustments are minimum compared to "test-first" approachs.

After both APIs/public interfaces and tests are firmed, then implementing them will be breeze, since we are guarded by 2 constraints: the "contracts" and the tests.

After the implementations done, the only thing need to do is adjusting the "assertions" in your tests against implementations, just to make sure. Tests change only when you change the API/public interfaces, and changing them should be rarely happening since it's breaking stability, probably you need version 2 for that. The refactoring will be safer since those 2 guards will validate your code and you know whether you meet the requirements and implement them correctly. The other benefit is to make sure your API's/interfaces's stability whenever your code are changing.

My experience with previous codebase I handled/maintained, the distinction between *Interfaces* and *Implementations* is blurred, anything can be changed, without anything really breaks, or if it breaks, it gave worrysome to your head where thing really broke. It's nightmare for next maintainer handling these kind of codes, because they deal with things already in production, but not much things guarding them from changes. 

Another cons on writing test later is that: *"it's already on production, and worked, why should I write tests for that?"*, demotivating the will of current and future devs/maintainers writing tests on existing codes. But, if the tests are included in development phase via <s>TDD</s>BDD, then it's really adding value to the end codebase. Devs are gonna test their code in the end, whether manually or automatically, atleast with <s>TDD</s>BDD you're reducing time for manual testing and guarding the code from regressions, automatically. <s>TDD</s>BDD is great complement for statically and strongly typed language because you get 3 guards in your code: type system, API/interface constraints, and tests. Maintaining these code will be ease the life of future devs/maintainers.

--@@--

*..CMIIW..*