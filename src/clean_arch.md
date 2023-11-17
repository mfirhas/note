# Clean Architecture

from book: *Clean Architecture A CRAFTSMAN’S GUIDE TO SOFTWARE STRUCTURE AND DESIGN* by Robert C. Martin.

## Two Values of Software System
- **Behaviour**: Stakeholders define requirements, swe implements them and fix any bugs.
- **Architecture**: Software's architecture to ease changes of requirements. Software must be easy to change.

Most people focus on Behaviour aspect, but what actually important is architecture, because requirements keep changing and the easeness of requirements change will speed up software development yet still meeting the said requirements and easier finding and fixing bugs.

## 3 Main Paradigms of Programming
- **Structured Programming**: By Edsger Wybe Dijkstra in 1968. Imperative programming model with imperative constructs `if/then/else` and `do/while/until`. *Structured programming imposes discipline on direct transfer of control*.
- **Object Oriented Programming**: by Ole Johan Dahl and Kristen Nygaard in 1966. Late binding of function calls to heaps. *Object-oriented programming imposes discipline on indirect transfer of control*.
- **Functional Programming**: By Alonzo Church in 1936 through his Lambda Calculus. Immutable. *Functional programming imposes discipline upon assignment.*


## Structured Programming

> *In 1955, having been a programmer for three years, and while still a student, Dijkstra concluded that
the intellectual challenge of programming was greater than the intellectual challenge of theoretical
physics. As a result, he chose programming as his long-term career.*

> *During his investigation, Dijkstra discovered that certain uses of goto statements prevent modules
from being decomposed recursively into smaller and smaller units, thereby preventing use of the
divide-and-conquer approach necessary for reasonable proofs.*

Structured Programming: using the good parts of `goto` feature, the imperative constructs, like `if/then/else` and `do/while`.

In 1968, Dijkstra wrote a letter titled "Go To Statement Considered Harmful"

From Dijkstra -> Tests: *are to show the presence of bugs, not the absence of them. We tests our program as much as we can until we failed to prove incorrectness*, then it's correct enough.


## Object-Oriented Programming
First form of OO is when Dahl and Nygaard before 1966 moved function call stack-frame to heap becoming function pointer.
The definition become: "Data + Behaviour". 
Then become: "Encapsulation + Inheritance + Polymorphism"
- Encapsulation: not unique to OO language as a general concept. C has actually encapsulation in form of header file.
  The removal of header file and using private,public and protected members as a way to encapsulate things actually weakens encapsulation C has.
- Inheritance: Prior to OOP "inheritance" was done through some kind of composition, becoming superset of something, even today composition still preferred over inheritance.
- Polymorphism: Pre-OO has somekind of inheritance in form of function pointers which have same signatures for different implementations.
  E.g. IO interface functions in UNIX having 5 API: `open`, `close`, `read`, `write`, and `seek`.
  This state that function pointer is the early form of OO's inheritance.

OO actually gives nothing new, it just make things in the past safer for implementations, especially in the hands of programmers not experienced enough with low level memory managements and things like pointers.

### Dependency Inversion
Previously the program dependency went from top level function(e.g. main) into lower level calling in each layer/level. Flow and dependencies go from top to bottom.

**Dependency Inversion**: the flow is reversed through the use of interface(bunch of function pointers) to define dependencies and used by top level as their calling signatures, and by implementors as calee's signatures. With inversion, dependencies go into interface, interface acts as implementation references for implementor, means it's implementor who depends on those references(interface), and caller who used to depends on implementor, now be the ones who declared those implementation references(interface), this interface also called as business rules.

OOP: *OO is the ability, through the use of polymorphism, to gain absolute control over every source code dependency in the system*.

The ability to loosely coupling software components like a plugins, those plugins implementations will refer to APIs defined in the interface.

> We can change implementation detail without client/caller aware of it and can load it into code/program directly because of the common interface shared, also can be load at runtime with dynamic linking strategy.


## Functional Programming
Immutability: Why matters? because: *All race conditions, deadlock conditions, and concurrent update problems are due to mutable variables. You cannot have a race condition or a concurrent update problem if no variable is ever updated. You cannot have deadlocks without mutable locks.*

Need to segregate between between mutable and immutable values in order to make immutability pragmatic. E.g. using transactional memory like atomic, other way is to implement ***Event Sourcing*** to record only the transaction, not the value resulted from it. If we calculate value from the transaction, there will be state need to be managed from previous transaction, it becomes stateful, becoming mutable, hence not functional enough.

**Conclusion**
- Structured programming is discipline imposed upon direct transfer of control.
- Object-oriented programming is discipline imposed upon indirect transfer of control.
- Functional programming is discipline imposed upon variable assignment.


## SOLID
> *The SOLID principles tell us how to arrange our functions and data structures into
classes, and how those classes should be interconnected. The use of the word “class”
does not imply that these principles are applicable only to object-oriented software.
A class is simply a coupled grouping of functions and data. Every software system
has such groupings, whether they are called classes or not. The SOLID principles
apply to those groupings.*

- SRP: Single Responsibility Principle
  - Each software modules/components must have **only 1 reason to change**.
  - Each module should only has one user/stakeholder/actor.
  - Each module should only be responsible to one user/stakeholder/actor.
  - Common mistake understanding this is saying each module/class only do one thing, which doesn't necessarily the case for SRP.
- OCP: Open/Close Principle
  Open for extension, Close for modification. Changes are made by adding new code, instead of modifying one.
  Separating system into different components(classes) with their own context(e.g. controller, presenters, database, models, etc)
  Dependencies go one way, A depends B, A knows about B, B doesn't need to know about A.
  Is about protecting higher dependencies from changes from lower dependencies, e.g. A depends on B, B is protected from changes in A.
  Utilizing interface to hide/information hiding, the most privileged component is the one rarely changes, and that many components depend on(e.g. interactor).
- LSP: Liskov Substitution Principle
  Is about subtypes, components are substitutable based on rules defined.
  When deriving a class, mind their functionalities and its impact on APIs. E.g. The Square/Rectangle Problem.
- ISP: Interface Segregation Principle
  Don't depends on things not used. Create interface for set of functionalities actually used by the client/caller, not the ones provided by implementors.
  Create small interfaces, only needed for each concrete components.
- DIP: Dependency Inversion Principle
  Implementation detail depends on specifications/requirements made by higher level components.
  Client/caller depends on abstractions(e.g. interface) not concrete implementations, and implementor implements those abstractions.

## Business Rules
- Entities: 
  - contains core business data and logics. 
  - Not related to other technical implementations(e.g. databases, IOs, etc)
  - Depend on nothing.
- Use Cases: 
  - Defines more elaborated business rules by utilizing entities. 
  - Entities know nothing about use cases, use cases know about entities. 
  - Usecases contain input and output data from outside(e.g. user inputs), process the input with its own logics and utilizing entities and give an output.
  - Depend on Entities
- Request and Response Models(DTOs)
  - Data model used by usecases as input and output.
  - Independent of anything, even entities.
  - Anemic: standalone and have no operations.

## The Clean Architecture
- Independent of Frameworks.
- Testable.
- Independent of the UI.
- Independent of the Database.
- Independent of any external agency.

### Layers
From high-level to low-level, dependencies go from low-level to high-level:
- Entities
- Use Cases
- Interface Adapters: controllers, APIs(rest api), grpc definitions and functions, models/DTOs, etc.
- Frameworks and Drivers(infrastructure): http frameworks, database, caches, any technical dependencies.

## Service Oriented Architecture and Microservice
SOA dan MS tidak menjamin dari total loose-coupling yang diharapkan dari dua arsitektur di atas, karena tetap ada *cross-cutting* concerns yang menyebabkan semua komponen baik itu monolith ataupun MS tetap harus diubah.

Solusi: Strategy Pattern
Object implementor terpisah dan comply ke set of common interfaces defined. Cukup menambahkan object implementor tersebut, dan membuat factory nya, maka tidak diperlukan perubahan di semua tempat.
Bisa menggunakan abstract class atau interface class sebagai boundaries untuk internal implementor dan memenuhi dependency inversion rule dimana abstract/interface class tersebut yang define rules(behaviours/functions/methods).

## Components
- Monolith: 
  - Produce 1 binary, 
  - The boundaries are all compiled into 1 binary/artifact,
  - Boundaries are in the level of function calls, abstract/interface segregation.
  - Run in one process
  - Changes can be made by re-compiling whole program, or inject component/object dynamically at runtime according to the boundaries.
- Services:
  - Multiple binaries
  - Independent deployments.
  - Run in one or multiple process, even in separate CPU/server/vm.
  - Communicate through common interface(e.g. http, rpc, etc)
  - Changes can be flexible as each service has its own concern and deployment pipeline without whole system recompile/redeploy.

## Tests
- Outermost of layers,
- Avoid tests coupling,
- Don't depend on volatile things, volatile things mostly are the IOs where they happen to be in infrastructure layers.
- Test coupling:
  - Structural coupling: 
    - Provide test apis to avoid this.
    - Test apis hide production code internal details so it's free to change, leaving test code unaffected.
    - Production codes tend to be more general, while Test codes tend to be more specific.
  - Separate Test code into its own module.

## Low-level details
- Database, Web, and Frameworks are low level details.
- Should be separated from core of the apps.
- Should not polluted system archs.
